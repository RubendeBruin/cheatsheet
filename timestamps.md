Pandas timestamps seem to be the most evolved.


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

### Adding date and time

For datetime
```
import datetime
datetime.datetime.combine(datetime.date(2011, 1, 1), 
                          datetime.time(10, 23))
```

for pandas
```
t_pandas = pd.Timestamp(data_start) + pd.TimedeltaIndex(low_t*1e9)
```

## Unix time (pyqtgraph)

#We need unix-time; unix-time is seconds since 1 January 1970

```
# start : datetime.datetime(2021, 5, 25, 21, 0, 0, 83393)

import time

unixtime = time.mktime(start.timetuple())
tx = unixtime + ts
```

and back

```
from datetime import datetime
datetime.fromtimestamp(xmin)
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
