Moving actors can be done using the vtkInteractorStyleTrackball**Actor**

```python
interactor = vtkRenderWindowInteractor()
interactor.SetRenderWindow(renderWindow)

style = vtkInteractorStyleTrackballActor()
interactor.SetInteractorStyle(style)
```

This gives a style in which actors can be moved (with the middle mouse button).

## Alternatively:

In a mouse-move event when moving (Blender: after pressing g)

```python
position2d = obj.GetEventPosition()  # <-- get mouse position

picker = vtk.vtkWorldPointPicker()
picker.Pick(position2d[0], position2d[1], 0, self.renderer)
position3d = picker.GetPickPosition()

view_normal = renderer.GetActiveCamera().GetViewPlaneNormal()
```

Now calculate the movement of the actor perpendicular to the view normal

