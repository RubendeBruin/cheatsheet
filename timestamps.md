use pandas timestamps. They seems to be the most evolved.


```
pd.Timestamp(anything)
```

will usually work.

It works for np.datetime64 and datetime objects.

Then, to convert to seconds use:

```
t_from_meas_start.total_seconds()
```

warning: do NOT use .seconds for this.

From 
- start-time and
- array of time-difference in seconds
to pandas:

```
t_pandas = pd.Timestamp(data_start) + pd.TimedeltaIndex(low_t*1e9)
```

## Matplotlib

Convert to matplotlib dates: ```time_mpl = date2num(python time)```
so from pandas to mpl do: ```time_mpl = date2num(time.to_pydatetime())```

Then to format:

```
import matplotlib.dates as mdates

myFmt = mdates.DateFormatter('%d %H:%M')
myLocator = mdates.MinuteLocator(0)           # tick where minutes = 0, so every hour!

ax.xaxis.set_major_formatter(myFmt)
ax.xaxis.set_major_locator(myLocator)
```
