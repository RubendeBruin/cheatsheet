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
```
