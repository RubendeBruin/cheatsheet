```python

from PySide2.QtWidgets import QMessageBox

QMessageBox.information(parent=self,
                                title = 'Done',
                                text = text,
                                button0=QMessageBox.Ok
                                )
                                
```

where parent needs to be an instance of QWidget

for node-editors: self.widget
