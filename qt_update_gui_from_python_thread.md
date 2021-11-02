This is an example to mix python threads with a Qt Gui where the qt-gui needs to be updated from the thread.

A simple callback works but is not safe. After a while the program crashes.

A work-around is to use a qt signal/slot conection. This seems to work

```python

from time import sleep

from PySide2 import QtWidgets
from PySide2.QtCore import Signal, Slot, QObject

app = QtWidgets.QApplication()

w = QtWidgets.QWidget()
layout = QtWidgets.QGridLayout()
label = QtWidgets.QLabel()
label.setText('test')

log_screen = QtWidgets.QPlainTextEdit()

layout.addWidget(label)
layout.addWidget(log_screen)
w.setLayout(layout)


# Create a signal-slot to update the gui
#
# Note: directly updating the gui crashes the program

@Slot(str)
def update_gui(txt):
    label.setText(txt)
    log_screen.appendPlainText(txt)

class Comminucator(QObject):   # slots need to be defined inside a class derived from QObject (ref: https://wiki.qt.io/Qt_for_Python_Signals_and_Slots)
     # Signals are runtime objects owned by instances, they are not class attributes:
    action = Signal(str)

com = Comminucator()
com.action.connect(update_gui)



def background_function(callback_signal, prefix, sleep_time):
    i = 0
    while True:
        sleep(sleep_time)
        i+=1
        callback_signal.emit(prefix + str(i))  # from the thread, emit the signal

# now use normal python threading
w.show()


from threading import Thread
new_thread = Thread(target=background_function,args=(com.action,"een ", 0.1))
new_thread.start()

new_thread_2 = Thread(target=background_function,args=(com.action,"twee ", 0.05))
new_thread_2.start()


app.exec_()

```


Or, same idea but better reusable:

```python
"""This works
"""

from time import sleep

from PySide2 import QtWidgets
from PySide2.QtCore import Signal, Slot, QObject

app = QtWidgets.QApplication()

w = QtWidgets.QWidget()
layout = QtWidgets.QGridLayout()
label = QtWidgets.QLabel()
label.setText('test')

log_screen = QtWidgets.QPlainTextEdit()

button = QtWidgets.QPushButton()
button.setText('Terminate')

layout.addWidget(label)
layout.addWidget(log_screen)
layout.addWidget(button)

w.setLayout(layout)


# signal definition
class SignalWrapper(
    QObject):  # slots need to be defined inside a class derived from QObject (ref: https://wiki.qt.io/Qt_for_Python_Signals_and_Slots)
    # Signals are runtime objects owned by instances, they are not class attributes:
    action = Signal(str)


class Reporter:

    terminate = False
    sleep_s = 0.1

    def __init__(self):
        self.on_feedback = None # assign to a QtSignal

    def feedback(self, txt: str):
        if self.on_feedback is not None:
            self.on_feedback.emit(txt)

    def run(self):
        i = 0
        while not self.terminate:
            i += 1
            sleep(self.sleep_s)
            self.feedback('progress: ' + str(i))

# Create a signal-slot to update the gui
#
# Note: directly updating the gui crashes the program

@Slot(str)
def update_gui(txt):
    label.setText(txt)
    log_screen.appendPlainText(txt)

com = SignalWrapper()
com.action.connect(update_gui)

report = Reporter()
report.on_feedback = com.action


report2 = Reporter()
report2.sleep_s = 0.2
report2.on_feedback = com.action

def terminate():
    report.terminate = True

button.pressed.connect(terminate)

# now use normal python threading
w.show()

from threading import Thread
new_thread = Thread(target=report.run)
new_thread.start()

new_thread = Thread(target=report2.run)
new_thread.start()


app.exec_()

```
