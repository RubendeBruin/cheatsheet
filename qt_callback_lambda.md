ref: https://forum.qt.io/topic/130735/different-behavior-of-pressed-vs-clicked-with-lambdas/3

```python
button.clicked.connect(lambda *args, a=i: print(f'clicked {a}'))
```
