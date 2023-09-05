Make this:

<img width="353" alt="image" src="https://github.com/RubendeBruin/cheatsheet/assets/34062862/ded858b8-4e66-4030-a3a1-da6d03d9ceaf">


```python
from PySide6 import QtWidgets as Qt
from PySide6.QtWidgets import QApplication

import vtk

from vedo import *

import sys

from vtk.qt.QVTKRenderWindowInteractor import QVTKRenderWindowInteractor

from vedo import Plotter, Cone


import sys
import matplotlib

matplotlib.use('Qt5Agg')

from PySide6.QtWidgets import QMainWindow, QApplication

from matplotlib.backends.backend_qt5agg import FigureCanvasQTAgg
from matplotlib.figure import Figure


class MplCanvas(FigureCanvasQTAgg):

    def __init__(self, parent=None, width=5, height=4, dpi=100):
        self.fig = Figure(figsize=(width, height), dpi=dpi)
        self.axes = self.fig.add_subplot(111)
        super(MplCanvas, self).__init__(self.fig)


class MainWindow(Qt.QMainWindow):
    def __init__(self, parent=None):

        Qt.QMainWindow.__init__(self, parent)
        self.frame = Qt.QFrame()
        self.vl = Qt.QVBoxLayout()
        self.vtkWidget = QVTKRenderWindowInteractor(self.frame)
        self.vl.addWidget(self.vtkWidget)

        self.sc = MplCanvas(self, width=5, height=4, dpi=100)
        self.sc.axes.plot([0, 1, 2, 3, 4], [10, 1, 20, 3, 40])

        self.vl.addWidget(self.sc)

        # create renderer and add the actors
        self.vp = Plotter(qt_widget=self.vtkWidget)

        self.vp += Box(pos = (10,-10,0), c = 'r')
        self.vp += Box(pos=(10, 10, 0), c = 'g')
        self.vp += Box(pos=(-10, -10, 0), c = 'b')
        self.vp += Box(pos=(-10, 10, 0), c = 'y')
        self.vp.show()

        # set-up the rest of the Qt window
        button = Qt.QPushButton("My Button makes the cone red")
        button.setToolTip('This is an example button')

        self.vp.interactor.AddObserver(vtk.vtkCommand.InteractionEvent, self.plot)

        button.pressed.connect(self.plot)

        self.vl.addWidget(button)
        self.frame.setLayout(self.vl)
        self.setCentralWidget(self.frame)
        self.show() # <--- show the Qt Window

    def plot(self, *args):
        self.sc.axes.cla()
        self.sc.draw()

        pos3d = [(10,-10,0),
        (10, 10, 0),
        (-10, -10, 0),
        (-10, 10, 0)]

        cols = 'r*','g*','b*','y*'

        camera = self.vp.renderer.GetActiveCamera()
        up = camera.GetViewUp()
        into = camera.GetViewPlaneNormal()
        right = np.cross(up, into)

        print(up)

        for p, c in zip(pos3d, cols):
            
            x = np.dot(p, right)
            y = np.dot(p, up)

            self.sc.axes.plot(x,y,c)

        self.sc.axes.set_aspect('equal')
        self.sc.draw()



    def onClose(self):
        #Disable the interactor before closing to prevent it
        #from trying to act on already deleted items
        self.vtkWidget.close()



if __name__ == "__main__":
    app = Qt.QApplication(sys.argv)
    window = MainWindow()
    app.aboutToQuit.connect(window.onClose) # <-- connect the onClose event
    app.exec_()

```
