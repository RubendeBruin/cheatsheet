Example of how to override the setter in a derived class

```python
    @property
    def parent(self) -> Frame or None:
        """Frame that the point is located on, determines the local axis system"""
        return super().parent

    @parent.setter # need to override because we did override getter
    def parent(self, value):
        super(Point, type(self)).parent.fset(
            self, value
        )  # https://bugs.python.org/issue14965
```       
