gumbel fitting



```python

from scipy.stats import gumbel_r

loc, scale = gumbel_r.fit(samples)
gp90 = gumbel_r.ppf(0.9, loc, scale) # P 90
```


    
