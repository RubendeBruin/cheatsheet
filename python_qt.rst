Tables
========

events ::

   table.itemChanged.connect(self.node_table_cell_edit_done)
   table.itemSelectionChanged.connect(self.selection_changed)
   
   table.currentRow()    
   table.currentColumn()  --> to give the cell that was changed
   

filling cells ::
    
   from PySide2.QtWidgets import QTableWidgetItem 
   
   tableWidget.setItem(row, col ,QtWidgets.QTableWidgetItem('contents'))
   tableWidget.setVerticalHeaderItem(row, QTableWidgetItem('vertical header content'))
   
   # with a combo-box
   cbx = QtWidgets.QComboBox()
   tableWidget.setCellWidget(row, 0, cbx)

   # with a color
   from PySide2.QtGui import QColor, QBrush
   col = QColor.fromRgb(254, 254, 254)
   tableWidget.item(row, col).setBackground(QBrush(col))
   
column size ::

   table.resizeColumnsToContents()
   table.horizontalHeader().setSectionResizeMode(QtWidgets.QHeaderView.Stretch) # does not work with dropdown box

Widgets events
===============

drop-down box (combobox)::

   combobox.currentIndexChanged.connect(self.action)


Tree view (item-based)
========================

Adding items::

   tree.clear()
   item = QTreeWidgetItem()
   item.setText(0,text)     # 0 is the column
   
   # add to root
   self.ui.tree.invisibleRootItem().addChild(item)
   
   # add to other item
   other_item.addChild(item)
   
getting items::

   tree.expandAll()
   
   self.treeView.selectedItems()[0].text(0)

events::

  tree.activated.connect(self.tree_select_node)  # fires when a user presses [enter]
  tree.doubleClicked.connect(self.tree_select_node)
  tree.itemClicked.connect(self.item_clicked)


Dialogs
=========

Init ::

   self.MainWindow = QtWidgets.QDialog()
   self.ui = Ui_DefaultMotionDialog()
   self.ui.setupUi(self.MainWindow)

Open/Close::

   self.MainWindow.exec_()

   self.MainWindow.close()


Applications
==============

::

   app = QApplication.instance()  # already existing instance

or create one:

::

   app = QtWidgets.QApplication()

VTK renderer and interactor shutdown
=======================================
 
shutdown::

    renwin.Finalize()
    iren.TerminateApp()
    
    if needed:
    ren = self.vtkWidget.GetRenderWindow()
    iren = ren.GetInteractor()
