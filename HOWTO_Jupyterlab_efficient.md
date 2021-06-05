# HOWTO use Jupyterlab efficiently

Jupyterlab is nice. But it can be made even better.

My setup is:

- Screen1 : Jupyter lab, notebook and console.
- Screen2 : PyCharm, for simultaneous development of the underlying functions or classes

Then use:

## 1. Autoreload

Enable the auto-reload extension. This makes that functions and modules are automatically re-loaded when changed.

```
%load_ext autoreload
%autoreload 2
```

## 2. Interactive plotting

Use PyQtGraph for plotting. This is the only package that can handle huge amounts of data smoothly. It is also possible to open a window with a plot and then update the plot in that window.

Enable the qt event-loop:
```
%gui qt
```

then:
```
import pyqtgraph as pg

pg.mkQApp() # Start an app

# make an empty window to plot in
plotw = pg.PlotWidget(axisItems = {'bottom': pg.DateAxisItem()})
plotw.showGrid(x=True, y=True)
plotw.show()

#We need unix-time; unix-time is seconds since 1 January 1970
unixtime = time.mktime(start.timetuple())
tx = unixtime + ts

plotw = pg.PlotWidget(axisItems = {'bottom': pg.DateAxisItem()})
plotw.showGrid(x=True, y=True)
plotw.addLegend()  # Add legend before adding lines
line = plotw.plot(tx, S0, pen=col[0], name='original')
```

to clear

```
plotw.clear()  # clear the window
```

## 3. Debugging!

The PyCharm debugger can connect to the ipython-kernel that Jupyter is running.
