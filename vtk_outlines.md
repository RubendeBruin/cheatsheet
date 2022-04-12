Add outlines to actors in vtk:

Option1: Static (no silhouette)

```python
ol = vtk.vtkFeatureEdges()
ol.SetColoring(False)  # does not seem to do anything
ol.SetInputConnection(tr.GetOutputPort())

ol.ExtractAllEdgeTypesOff()
ol.BoundaryEdgesOn()
ol.SetFeatureAngle(25)
ol.FeatureEdgesOn()
```

Option2: Dynamic (with silhouette)

```python
ol = vtk.vtkPolyDataSilhouette()
ol.SetInputConnection(tr.GetOutputPort())
ol.SetEnableFeatureAngle(True)
ol.SetCamera(vtk_camera)
ol.SetBorderEdges(True)
```

Then
```
mapper = vtk.vtkPolyDataMapper()
mapper.SetInputConnection(ol.GetOutputPort())
mapper.ScalarVisibilityOff ()  # No colors!

actor = vtk.vtkActor()
actor.SetMapper(mapper)
actor.GetProperty().SetColor(0, 0, 0)
actor.GetProperty().SetLineWidth(1)
```
