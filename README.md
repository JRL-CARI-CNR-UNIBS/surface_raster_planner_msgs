# surface_raster_planner_msgs

ROS 2 interfaces used by the `surface_raster_planner` package (separate repository).

## Services

### `ComputeTrajectory.srv`

Computes the raster (serpentine) path over the loaded mesh (service `/raster_planner_node/compute_trajectory`).

| Field | Type | Description |
|---|---|---|
| `row_step` (request) | `float64` | [m] distance between consecutive rows, along the short side of the surface |
| `keep_tool_orientation` (request) | `bool` | `true`: the tool x axis always points along the long side (`e1`), also on the return rows. `false`: the x axis follows the travel direction, so it flips on every row |
| `success` (response) | `bool` | outcome |
| `message` (response) | `string` | description of the outcome or of the error |
| `poses` (response) | `geometry_msgs/PoseArray` | tool poses in the mesh frame: z = surface normal, x = travel direction |
| `jump_indices` (response) | `uint32[]` | sorted indices, never 0, where a new continuous segment starts. Number of segments = `len(jump_indices) + 1` |
| `discarded_points` (response) | `uint32` | raster points whose ray did not hit the mesh |

Example:

```bash
ros2 service call /raster_planner_node/compute_trajectory surface_raster_planner_msgs/srv/ComputeTrajectory \
    "{row_step: 0.02, keep_tool_orientation: false}"
```

### `LoadMesh.srv`

Loads a mesh from file (service `/raster_planner_node/load_mesh_from_file`).

| Field | Type | Description |
|---|---|---|
| `filename` (request) | `string` | file path; supported formats: `.ply`, `.obj`, `.vtk`, `.stl` |
| `frame_id` (request) | `string` | frame of the mesh vertices; if empty, the node parameter `mesh_frame` is used |
| `success` (response) | `bool` | outcome |
| `message` (response) | `string` | description of the outcome or of the error |

Example:

```bash
ros2 service call /raster_planner_node/load_mesh_from_file surface_raster_planner_msgs/srv/LoadMesh \
    "{filename: '/tmp/surface.ply', frame_id: 'world'}"
```

## Authors

Developed by **CARI-JRL**.

Developer and maintainer: Manuel Beschi (<manuel.beschi@unibs.it>)

## License

Apache License 2.0, see [LICENSE](LICENSE).

Copyright 2026 CARI-JRL
