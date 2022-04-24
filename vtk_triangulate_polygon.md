Snippet to triangulate a polygon.
This is usefull for display of non-convex polygons in 3d, for example the surface area of a tank or the stern of a ship.

```
n = len(poly)

points = vtk.vtkPoints()

for p in poly:
    points.InsertNextPoint(p[0], p[1], p[2])

# Create the polygon
polygon = vtk.vtkPolygon()
polygon.GetPointIds().SetNumberOfIds(n)
for i in range(n):
    polygon.GetPointIds().SetId(i, i)

polygons = vtk.vtkCellArray()
polygons.InsertNextCell(polygon)

polydata = vtk.vtkPolyData()
polydata.SetPoints(points)
polydata.SetPolys(polygons)

# Triangulate the grid points
triangulate = vtk.vtkTriangleFilter()
triangulate.SetInputData(polydata)
triangulate.Update()

# get the output data
data = triangulate.GetOutput()

points = []
faces = []

for i in range(data.GetNumberOfPoints()):
    points.append(data.GetPoint(i))

for i in range(data.GetNumberOfCells()):
    cell = data.GetCell(i)
    if cell.GetNumberOfPoints() != 3:  # just check
        raise ValueError('Face should have three vertices (trianglefilter), but number of points is not 3')
    faces.append((cell.GetPointId(0),  # Note: we know that these are triangles
                     cell.GetPointId(1),
                     cell.GetPointId(2)))
```
