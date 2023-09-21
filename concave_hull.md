#Concave hulls

Concave hulls are not unique.
There are numerous methods and packages. 

- alpha shapes
- concavehull with chi factor. (c++ implementation, works but does not yield the required results). Does not connect the middle point even with extreme chi factors
- There is also a nearest-neightbour/angle algorithm with a k parameter (determining the number of nearest points).


## Alpha shapes

Using the package `alpha_shapes`:

```python
from alpha_shapes import Alpha_Shaper, plot_alpha_shape
import numpy as np
import matplotlib.pyplot as plt


pts = [(0,0),(1,1),(0,1),(1,0),(0.7, 0.5)]
points = np.array(pts)


for alpha in (1.6, 1.7, 1.8, 1.9, 2):

    fig, ax = plt.subplots()

    shaper = Alpha_Shaper(points)

    alpha_shape = shaper.get_shape(alpha=alpha)

    ax.scatter(*zip(*points))
    plot_alpha_shape(ax, alpha_shape)
    ax.set_title(str(alpha))


plt.show()
```

The shape is determined based on an "alpha" paremeter:

<img width="1408" alt="image" src="https://github.com/RubendeBruin/cheatsheet/assets/34062862/f56ed5c5-e348-4fb1-920f-afbc3a64236f">

It seems to be easiest to determine alpha such that every point is visited once.
