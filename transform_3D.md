Use Homogeneous Coordinates

position: (x,y,z,1)
direction: (x,y,z,0)

rotate using rotated = T * coordinate

where T is a 4x4 matrix the unit-vectors as first three columns and the position as fourth column.
Last row = [0,0,0,1]

```python
 # unit-vectors: dx,dy,dz
 # position: (x,y,z)

 T = np.zeros((4,4))
 T[0:3,0] = dx
 T[0:3,1] = dy
 T[0:3,2] = dz
 T[0:3,3] = (x,y,z)
 T[3,3] = 1

 # transform position
 pos = np.matmul(T, (*pos,1))[:3]  # set 1 on last entry of input vector, ignore output vector[-1]
 # transform direction
 dir = np.matmul(T, (*dir,0))[:3]  # set 0 on last entry of input vector, ignore output vector[-1]

```
