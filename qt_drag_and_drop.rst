*Attaching mime data*

*Internal drag/drop in a treeview*

::

  class NodeTreeWidget(QtWidgets.QTreeWidget):

      def dragEnterEvent(self, event):
          if event.source() is not self:
              print("Not accepting external data")
              event.setDropAction(Qt.IgnoreAction)
              return
          else:
              event.accept()

      def dragMoveEvent(self, event):
          if event.source() is not self:
              print("Not accepting external data")
              event.setDropAction(Qt.IgnoreAction)
              return
          else:
              event.accept()

      def dropEvent(self, event):
          if event.source() is not self:
              print("Not accepting external data")
              event.setDropAction(Qt.IgnoreAction)
              return

          dragged_name = event.mimeData().text()

          # dropped onto
          point = event.pos()
          drop_onto = self.itemAt(point)

          if drop_onto is None:
              drop_onto_name = None
          else:
              drop_onto_name = drop_onto.text(0)

          print('dragged {} onto {}'.format(dragged_name,drop_onto_name))
          event.setDropAction(Qt.IgnoreAction)

          self.parentcallback(dragged_name, drop_onto_name, event)

      def startDrag(self, supportedActions):

          dragged = self.selectedItems()[0]
          mimeData = QMimeData()
          mimeData.setText(dragged.text(0))

          drag = QDrag(self)
          drag.setMimeData(mimeData)
          drag.exec_(supportedActions=supportedActions, defaultAction=Qt.MoveAction)
          
* Attach on drag-out

::

  class TreeWithDragOut(QtWidgets.QTreeWidget):

      def mouseMoveEvent(self, event):

          # we should have only a single item selected
          item = self.selectedItems()
          if len(item) != 1:
              return

          item = item[0]

          # item should not have a parent (should not be a value)
          if item.parent() is None:
              drag = QDrag(self)
              mime = QtCore.QMimeData()

              text = item.text(0)
              if text[0]=='.':
                  text = "s['{}']{}".format(self.nodename, text)

              mime.setText(text)
              drag.setMimeData(mime)
              drag.start(QtCore.Qt.MoveAction)
              

*Accepting a drop

  self.ui.editEvaluate.acceptDrops()
  
  dragEnterEvent(event)
  
  if (event->mimeData()->hasFormat("text/plain"))
        event->acceptProposedAction();

*Demo with two list-views

::

  from PySide2 import QtWidgets
  from PySide2.QtCore import QMimeData, Qt
  from PySide2.QtGui import QDrag

  app = QtWidgets.QApplication()

  form = QtWidgets.QMainWindow()
  contents = QtWidgets.QWidget()
  form.setCentralWidget(contents)
  layout = QtWidgets.QVBoxLayout(form)
  contents.setLayout(layout)


  list = QtWidgets.QListWidget()
  list.addItems(["een","twee"])
  layout.addWidget(list)

  table = QtWidgets.QListWidget()
  layout.addWidget(table)



  # --------- create and attach drag-out on list --------

  list.setDragEnabled(True)

  def startDrag(supportedActions):
      mimeData = QMimeData()
      item = list.currentItem().text()
      mimeData.setText(item)

      drag = QDrag(list)
      drag.setMimeData(mimeData)
      drag.exec_(supportedActions=supportedActions, defaultAction=Qt.MoveAction)

  list.startDrag = startDrag

  # ----------- enable on-drop on table -----

  table.setAcceptDrops(True)

  def dropAction(event):
      table.addItem(event.mimeData().text())

  def dragEnterEvent(event):
      event.accept()

  def dragMoveEvent(event):
      event.accept()


  table.dropEvent = dropAction
  table.dragEnterEvent = dragEnterEvent
  table.dragMoveEvent = dragMoveEvent


  form.show()
  app.exec_()


