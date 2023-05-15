- super() needs to be called in every class that is derived from somethings (ie is a subclass)
- super() is not to be called in base-classes. It would result in an argument error when calling init of object with arguments

```
from abc import ABC

class A(ABC):
    def __init__(self, test):
        print("A")
        super().__init__(test)  # <-- We do need to call super here as we derive from ABC

class B(A):
    def __init__(self, test):
        print("B")
        super().__init__(test)

class C():
    def __init__(self, test):
        print('C')
        # super().__init__(test)  # <-- No need to call super here


class D(B,C):
    def __init__(self, test):
        print('D')
        super().__init__(test)


D('test')
```
