coordinate = vtk.vtkCoordinate()
coordinate.SetCoordinateSystemToWorld()
coordinate.SetValue(pos3d[0], pos3d[1], pos3d[2])
displayCoor = coordinate.GetComputedDisplayValue(renderer)
