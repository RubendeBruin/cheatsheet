Layout do not have children. To get all the widgets in a layout use:

```python
widgets = [self.layout.itemAt(i).wid for i in range(self.layout.count())] 
```
