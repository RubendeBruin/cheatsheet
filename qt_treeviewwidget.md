Filling:

Create an item

```pyhton
item = QTreeWidgetItem()
item.setText(0, text)
```

Add to root-item or to other item:
```pyhton
treeView.invisibleRootItem().addChild(item)
item.addchild(item)
```


How to get all items:

```python
closed_items = []

def walk_node(item, store_here):
    for i in range(item.childCount()):
        child = item.child(i)
        if child.childCount() > 0:
            if not child.isExpanded():
                store_here.append(child.text(0))
            walk_node(child, store_here)

walk_node(self.treeView.invisibleRootItem(), closed_items)
```
        
restore the closed-state of the items in the tree:

```python
# restore closed nodes state
def close_nodes(item, closed):
    for i in range(item.childCount()):
        child = item.child(i)
        if child.text(0) in closed_items:
            child.setExpanded(False)
        if child.childCount() > 0:
            close_nodes(child, closed)
close_nodes(self.treeView.invisibleRootItem(), closed_items)                    
```

Enabling Drag/Drop:

```python
def startDrag(self, supportedActions):

dragged_index = self.ui.treeWidget.selectedItems()[0].section_index
mimeData = QMimeData()
mimeData.setText(str(dragged_index))

drag = QDrag(self.ui.treeWidget)  # <-- this is the "source" of the event
drag.setMimeData(mimeData)

drag.exec_(supportedActions=supportedActions)

def tvdragEnterEvent(self, event):
if event.source() is self.ui.treeWidget:
    event.accept()
else:
    event.setDropAction(Qt.IgnoreAction)


def tvdragMoveEvent(self, event):
if event.source() is self.ui.treeWidget:
    event.accept()
else:
    event.setDropAction(Qt.IgnoreAction)

def tvdropEvent(self, event):

print('drop event')

if event.source() is not self.ui.treeWidget:
    # print("Not accepting external data")
    event.setDropAction(Qt.IgnoreAction)
    return

dragged_index = int(event.mimeData().text())

# dropped onto
point = event.pos()
drop_onto = self.ui.treeWidget.itemAt(point)

if drop_onto is None:
    drop_onto_name = None
    event.setDropAction(Qt.IgnoreAction)
    return
else:
    drop_onto_index = drop_onto.section_index

print(f'Dropped {dragged_index} onto {drop_onto_index}')
```


Events for selecting:

Selection changed (good for single selection)
```python
def selection_changed(self, *args):
    if self.ui.treeWidget.selectedItems():
        print(f'section {self.ui.treeWidget.selectedItems()[0].section_index}')
```

otherwise:

```python
self.treeView.activated.connect(
            self.tree_select_node
        )  # fires when a user presses [enter]
self.treeView.doubleClicked.connect(self.tree_select_node)
self.treeView.itemClicked.connect(self.item_clicked)
```
