All possible combinations using meshgrid

```python
grid = np.meshgrid(
            *[(True, False) for i in range(n)])  # this produces numpy booleans, which can not be yaml-ized

n_combinations = len(grid[0].flat)

combination = []
for i in range(n_combinations):
    combination.append([a.flat[i] for a in grid])
```
