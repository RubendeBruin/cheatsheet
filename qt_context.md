self.treeView.customContextMenuRequested.connect(self.rightClickTreeview)

# VTK:
globLoc = self.ui.frame3d.mapToGlobal(point)

# widgets

globLoc = self.treeView.mapToGlobal(point)  

menu = QtWidgets.QMenu()

menu.addAction("Edit {}".format(node_name), edit)

menu.exec_(globLoc)

evt:
menu.exec_(QCursor.pos())
