# Tetrahedron rotation around symmetry axes

## Rig
1. Enable add-on: `Add Mesh: Extra Objects` (Platonic Solids).
2. Add **Tetrahedron**.
3. Pick a pair of opposite edges; for each edge:
   - Select its two vertices, place 3D cursor at midpoint, add Empty at cursor.
   - Name them `start_empty` and `end_empty`.
4. Add an **Empty Arrow** named `axis_arrow`.
5. Constrain `axis_arrow` to define the axis (start → end), then bake:
```json
{
  "constraints": [
    { "type": "COPY_LOCATION", "name": "CopyLocation_Start", "target": "start_empty" },
    { "type": "TRACK_TO", "name": "TrackTo_End", "target": "end_empty", "track_axis": "TRACK_Z", "up_axis": "UP_Y" }
  ],
  "bake": { "apply_visual_transform": true }
}
```
6. Set `axis_arrow` rotation mode to **Quaternion (WXYZ)**.
7. Make the tetrahedron follow only rotation from `axis_arrow`:
```json
{
  "object": "tetrahedron",
  "constraints": [
    {
      "type": "CHILD_OF",
      "name": "ChildOf_AxisArrow",
      "target": "axis_arrow",
      "use_location_x": false,
      "use_location_y": false,
      "use_location_z": false,
      "use_rotation_x": true,
      "use_rotation_y": true,
      "use_rotation_z": true,
      "use_scale_x": false,
      "use_scale_y": false,
      "use_scale_z": false,
      "set_inverse": true
    }
  ]
}
```
## Animation

### Animate along an axis
1. Select the corresponding `axis_arrow`
2. Ensure **Rotation Mode = Quaternion (WXYZ)**
3. Insert rotation keyframes on the local axis Z
4. Set keyframe interpolation to **Linear**

### Multiple axes
- Create one `axis_arrow` per rotation axis (`axis_arrow_A`, `axis_arrow_B`, `axis_arrow_C`)
- Apply visual transform and set each arrow to Quaternion rotation like before
- Keep all **Child Of** constraints on the tetrahedron enabled at all times

```json
{
  "constraints": [
    { "type": "CHILD_OF", "name": "Axis_A", "target": "axis_arrow_A", "set_inverse": true },
    { "type": "CHILD_OF", "name": "Axis_B", "target": "axis_arrow_B", "set_inverse": true },
    { "type": "CHILD_OF", "name": "Axis_C", "target": "axis_arrow_C", "set_inverse": true }
  ]
}
```

### Animate the sequence
- Animate only **one axis_arrow at a time**
- Ensure rotations do not overlap in time
- The tetrahedron follows the active axis automatically



