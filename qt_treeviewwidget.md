How to get all items:

```python
# get expanded / closed sate of the elements currently in the list
        closed_items = []
        def walk_node(item, store_here):
            for i in range(item.childCount()):
                child = item.child(i)
                if child.childCount() > 0:
                    if not child.isExpanded():
                        store_here.append((item.text(0), child.text(0)))
                    walk_node(child, store_here)

        walk_node(self.ui.tree.invisibleRootItem(), closed_items)
        ```
