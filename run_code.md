Execute python code within a script and keep the result

```python
# See: https://stackoverflow.com/questions/1463306/how-does-exec-work-with-locals
# and: https://bugs.python.org/issue4831
ldict = locals()

try:
    exec(code, globals(), ldict)
except Exception as E:
    for i,line in enumerate(code.split('\n')):
        print(f'{i}: {line}')
    print(E)
    raise(E)
```
