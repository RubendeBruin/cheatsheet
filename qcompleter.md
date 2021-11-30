Adding a QCompleter to a combobox



```python
# set QCompleter
completer = QCompleter(names)
completer.setCaseSensitivity(Qt.CaseInsensitive)
completer.setModelSorting(QCompleter.UnsortedModel)
completer.setFilterMode(Qt.MatchContains)

self.ui.cbCombobox.setCompleter(completer)
```
