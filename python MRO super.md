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


if multiple classes derive from ABC then insert an intermediate class:

```
from abc import ABC, abstractmethod

# === define an intermediate class that strips the constructor arguments ===

class myABC(ABC):
    def __init__(self, test):
        print("myABC")
        super().__init__() # <-- call super without arguments


#----


class A(myABC):
    def __init__(self, test):
        print("A")
        super().__init__(test)  # <-- We do need to call super here as we derive from ABC

    @abstractmethod
    def foo(self):
        pass


class B(A):
    def __init__(self, test):
        print("B")
        super().__init__(test)

class C(myABC):
    def __init__(self, test):
        print('C')
        super().__init__(test)

    @abstractmethod
    def bar(self):
        pass


class D(B,C):
    def __init__(self, test):
        print('D')
        super().__init__(test)

    def foo(self):
        print('foo')

    def bar(self):
        print('bar')


D('test')
```
