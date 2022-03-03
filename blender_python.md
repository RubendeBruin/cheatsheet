Change the color of an material with BSDF shader:

```python
material = bpy.context.active_object.active_material
BSDF_node = material.node_tree.nodes['Principled BSDF']
BSDF_node.inputs['Base Color'].default_value = (254,2,3,1)
```
