To create and animate a wave-response from a spectrum using ifft.

See : https://numpy.org/doc/stable/reference/routines.fft.html for fft/ifft details
See : https://ocw.tudelft.nl/wp-content/uploads/OffshoreHydromechanics_Journee_Massie.pdf 

<img width="478" alt="image" src="https://github.com/RubendeBruin/cheatsheet/assets/34062862/cfb7def0-5370-4c73-b8bc-7c5dda4929f7">

To get the desired output resulution the number of input points needs to be fit 

```python
import waveresponse as wr
import numpy as np
import matplotlib.pyplot as plt


freqs = np.linspace(0.01, 4, num=50)

spec = wr.JONSWAP(freq=freqs, freq_hz=False)
f, S = spec(hs=0.5, tp=4, gamma=3.3)

t_max = 1000 # seconds
dt = 1/60 # seconds
N = int(t_max / dt)
t_max = N * dt


# create a omega vector that is compatible with the requested time range
d_omega = 2 * np.pi / t_max
omega = np.arange(0, N) * d_omega

# interpolate the spectrum to the omega vector - pad with zeros
A = np.interp(omega, f, S, left=0, right=0)

# define some random phase angles
random_phase = np.random.uniform(0, 2 * np.pi, len(omega))
phase = random_phase

# create the complex spectrum
Amp = np.sqrt(2*A*d_omega)
Sc  = Amp * np.exp(-1j * phase)

# create time-realisation from complex spectrum
r = N* np.fft.ifft(Sc).real
t = np.linspace(0, t_max, N)


# produce in x
# assume deep water

# from MaFreDo import wavelength

waveL = 2 * np.pi * 9.81 / omega ** 2

x = np.linspace(0, 40, num=100)
Elv = []
# Elv2 = []

for downwave_distance in x:

    # calculate the phase offset due to the down-wave distance
    # the phase is defined a lagging, so a positive down-wave distance results
    # in a positive phase offset

    phase_offset = 2 * np.pi * (downwave_distance / waveL)

    # r2 = N * np.fft.ifft(Amp * np.exp(-1j * (phase+phase_offset))).real
    phase_shift = np.exp(-1j * phase_offset)

    r = N * np.fft.ifft(Sc * phase_shift).real

    Elv.append(r)
    # Elv2.append(r2)

# plot

import matplotlib.animation as animation

fig, (ax_top, ax) = plt.subplots(2,1)
ax.equal_aspect = True


ax_top.plot(t,r)

# check the statistics of the generated signal
std = np.std(r)
from scipy.signal import find_peaks
i_peaks, _ = find_peaks(r, height=0)

ax_top.plot(t[i_peaks], r[i_peaks], 'x')

n_peaks = len(i_peaks)
Taverage = np.mean(np.diff(t[i_peaks]))

ax_top.set_title(f'std = {std:.2f} --> Hs = {4*std:.2f}\nTaverage = {Taverage:.2f}')


line, = ax.plot(x, np.sin(x))
title = ax.set_title('Time = 0')
ax.axis('equal')


def animate(i):
    y = [Elv[ix][i] for ix in range(len(x))]
    # y2 = [Elv2[ix][i]+0.1 for ix in range(len(x))]
    line.set_ydata(y)  # update the data.
    # line2.set_ydata(y2)  # update the data.
    title.set_text(f'Time = {i*dt:.2f}')
    return line, title


ani = animation.FuncAnimation(
    fig, animate, interval=dt*1000, blit=True)

# To save the animation, use e.g.
#
# ani.save("movie.mp4")
#
# or
#
# writer = animation.FFMpegWriter(
#     fps=15, metadata=dict(artist='Me'), bitrate=1800)
# ani.save("movie.mp4", writer=writer)

plt.show()
#

```
