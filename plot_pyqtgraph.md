pyqtgraph lets you create interactive graphs from a jupyter notebook as follows:

```
%gui qt

import pyqtgraph as pg

import time

#We need unix-time; unix-time is seconds since 1 January 1970
unixtime = time.mktime(start.timetuple())
tx = unixtime + ts


w = pg.PlotWidget(axisItems = {'bottom': pg.DateAxisItem()})
w.addLegend()  # Add legend before adding lines
line = w.plot(tx, channels['PU'], pen='g', name='green')
line = w.plot(tx, channels['AU'], pen=(100,100,0), name='ugly green')
w.showGrid(x=True, y=True)

w.show()
```

notes:
1. Add legend before adding lines
2. Look at the example browser for more options


### Change background color

```
## Switch to using white background and black foreground
pg.setConfigOption('background', 'w')
pg.setConfigOption('foreground', 'k')
```

### subplots with linked axes

```
lay = pg.GraphicsLayoutWidget(show=True, title="Whooo!")
p1 = lat.addPlot(name="UpperT", title="Upper", row=1, col=1, colspan=2, axisItems={'bottom': pg.DateAxisItem()})
p1.plot(x=x, y=y, pen='k', name='Blaat')

p2 = lat.addPlot(name="UpperT", title="Upper", row=2, col=1, colspan=2, axisItems={'bottom': pg.DateAxisItem()})
p2.plot(x=x, y=y, pen='k', name='Blaat')

p2.setXLink(p1) 
```



