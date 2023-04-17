Example of an editable QTableView using a model:

Based on https://www.pythonguis.com/tutorials/qtableview-modelviews-numpy-pandas/

Note: values that are returned shall be a string.
Do convert floats to a string with decimals to ensure that they are recognized as float

```
if isinstance(value, float):
            # Render float to 2 dp
            return "%.2f" % value
```            

```python
import sys
from PySide2 import QtCore, QtWidgets, QtGui
from PySide2.QtCore import Qt


class TableModel(QtCore.QAbstractTableModel):
    def __init__(self, data):
        super(TableModel, self).__init__()
        self._data = data

    def data(self, index, role):
        if role == Qt.DisplayRole or role == Qt.EditRole:
            # See below for the nested-list data structure.
            # .row() indexes into the outer list,
            # .column() indexes into the sub-list
            return self._data[index.row()][index.column()]
        elif role == Qt.BackgroundRole:
            value = int(self._data[index.row()][index.column()])
            return QtGui.QColor.fromRgb(255,int(value),int(value))

    def flags(self, index):
        return Qt.ItemIsSelectable | Qt.ItemIsEnabled | Qt.ItemIsEditable | Qt.ItemNeverHasChildren

    def rowCount(self, index):
        # The length of the outer list.
        return len(self._data)

    def columnCount(self, index):
        # The following takes the first sub-list, and returns
        # the length (only works if all rows are an equal length)
        return len(self._data[0])

    def setData(self, index, value, role=None):
        print(index, value)
        if float(value)>0:
            self._data[index.row()][index.column()] = value
            self.dataChanged.emit(index, index, Qt.DisplayRole) # last argument is optional
            return True
        else:
            return False
            
    def headerData(self, section, orientation, role):
        # section is the index of the column/row.
        if role == Qt.ItemDataRole.DisplayRole:
            if orientation == Qt.Orientation.Horizontal:
                return str(self._data.columns[section])

            if orientation == Qt.Orientation.Vertical:
                return str(self._data.index[section])

class MainWindow(QtWidgets.QMainWindow):
    def __init__(self):
        super().__init__()

        self.table = QtWidgets.QTableView()

        data = [
          [4, 9, 2],
          [1, 0, 0],
          [3, '', 0],
          [3, 3, 2],
          [7, 8, 9],
        ]

        self.model = TableModel(data)
        self.table.setModel(self.model)

        self.setCentralWidget(self.table)

        self.table.selectionModel().currentChanged.connect(lambda a,b : print(f'cell changed {a}, {b}'))

        # self.table.verticalHeader().setVisible(False)
        # self.table.horizontalHeader().setVisible(False)


app=QtWidgets.QApplication(sys.argv)
window=MainWindow()
window.show()
app.exec_()
```
