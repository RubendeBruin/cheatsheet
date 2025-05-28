ref: https://forum.qt.io/topic/130735/different-behavior-of-pressed-vs-clicked-with-lambdas/3

```python
button.clicked.connect(lambda *args, a=i: print(f'clicked {a}'))
```

background:

The clicked signal is overload so it accepts 2 signatures where it can send a bool or not. The default signature depends on the library, in this case it seems that PySide2 by default does not send the "checked" parameter, unlike PyQt5 that does.

The solution is to indicate the signature:
```python
button.clicked[bool].connect(lambda checked, a=a: self.button_clicked(a))
```
