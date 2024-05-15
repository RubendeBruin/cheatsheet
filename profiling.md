python profiling using cProfile

```python

from cProfile import Profile
from pstats import SortKey, Stats

with Profile() as profile:
    er = calculate_elemental_RAOs(s, omegas=None, directions=[0,45,90,135,180,225,270,315])

    Stats(profile).sort_stats(SortKey.CUMULATIVE).print_stats()
```
