Plotting wavelets

```python
from scipy.signal import cwt, morlet2

# note: morlet2 is morlet but then made suitable for the cwt command

def plotwavelet(t, signal, dt, cmap = 'Blues'):
    width = np.arange(1,100)
    R = cwt(signal, morlet2, width)
    plt.figure(figsize = [16,6])
    # extent : floats (left, right, bottom, top)
    w = 5   # standard w0 for morlet2 = 5
    fac = 4 * np.pi  / (w + np.sqrt(2+w**2)) # conversion factor
    extend = (min(t), max(t), 100*fac * dt, 0)
    plt.imshow(np.power(abs(R),2),  cmap=cmap, aspect='auto', extent =extend)
    plt.xlabel('Time [s]')
    plt.ylabel('Period [s]')
    
    return R
```
