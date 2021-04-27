Obtain the natural period and damping ratio from a measured signal.

Note: clean and select number of peaks to use

```python

from scipy.signal import find_peaks
import numpy as np

signal = np.array(########)

# set mean to zero
signal = signal - np.mean(signal)

inds = find_peaks(signal)[0]

# use first 10 peaks
inds = inds[:10]

plt.plot(t, signal)
plt.plot(t[inds],signal[inds],'r*')

P = signal[inds]

delta = np.log(P[:-1] / P[1:])

damping_ratio = delta / (np.sqrt(4*np.pi**2 + delta**2))

dr = np.mean(damping_ratio)

Tn = np.diff( t[inds])
Tn = np.mean(Tn)

print(f'Daming ratio = {100*np.mean(damping_ratio):.2f}%')
print(f'Natural period = {Tn:.2f} s')
```
