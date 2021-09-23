```python
import PySide2
from PySide2.QtCore import Qt, QPointF
from PySide2.QtGui import QBrush, QPen, QPolygonF
from PySide2.QtWidgets import QDialog, QApplication, QGraphicsView, QVBoxLayout, QGraphicsScene, QGraphicsPolygonItem

a = QApplication()
d = QDialog()

gf = QGraphicsView()
layout = QVBoxLayout()
layout.addWidget(gf)
d.setLayout(layout)

green_brush = QBrush(Qt.green)
yellow_brush = QBrush(Qt.yellow)
blue_pen = QPen(Qt.blue)
blue_pen.setWidth(4)

scene = QGraphicsScene()

def clik(event, text):
    poly.setBrush(yellow_brush)
    print(text)

# Can not add events to these rects...?
rect = scene.addRect(100,0,80,100, blue_pen, green_brush)
rect = scene.addRect(500,400,180,200, blue_pen, green_brush)

# But can add a event items added in this way (without the convenience methods)
p1 = QPointF(0,0)
p2 = QPointF(100,100)
p3 = QPointF(0,200)
p4 = QPointF(200,200)
p = QPolygonF([p1,p2,p3])

poly = QGraphicsPolygonItem(p)
poly.setBrush(green_brush)
scene.addItem(poly)

p = QPolygonF([p4,p2,p3])

poly2 = QGraphicsPolygonItem(p)
poly2.setBrush(green_brush)
scene.addItem(poly2)

poly.mousePressEvent = lambda x , text='een' : clik(x, text)
poly2.mousePressEvent = lambda x , text='twee' : clik(x, text)


scene.addItem(rect)

gf.setScene(scene)
bounding_rect = scene.sceneRect()
gf.fitInView(bounding_rect)

def update(event):
    gf.fitInView(bounding_rect)
    print('update')

d.resizeEvent = update

d.show()
a.exec_()
```
