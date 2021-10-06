```python

from PySide2.QtWidgets import QMessageBox

QMessageBox.information(self.widget,
                                'Done',
                                text,
                                QMessageBox.Ok
                                )
                                
```

where parent needs to be an instance of QWidget

for node-editors: self.widget
