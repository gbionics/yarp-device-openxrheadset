# OpenXrFrameViz

A visualization utility that reads transform frames published via the YARP `IFrameTransform` interface and displays them in a 3D viewer using iDynTree's `Visualizer`.

The utility automatically discovers all frames connected to the root frame and renders them as labeled coordinate frames, updating in real time.

Note: this utility can also be used with any transform source that can be accessed via the YARP `IFrameTransform` interface, not just OpenXR frames.
The name "OpenXrFrameViz" is used here because it was developed primarily for visualizing OpenXR frames, but it is not limited to that use case.

---

## Usage

```bash
OpenXrFrameViz [options]
```

---

## Configuration Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `name` | string | `"OpenXrFrameViewer"` | The name used as a prefix for YARP ports opened by the utility. |
| `tf_root_frame` | string | `"openxr_origin"` | The root transform frame. Only frames that can be transformed relative to this frame are displayed. |
| `z_up` | bool (flag) | `false` | If set (or set to `true`), uses the standard Z-up convention. Otherwise, uses the OpenXR convention where +Y is up and -Z is forward. |
| `frame_length` | double | `0.5` | The length of the coordinate frame axes displayed in the visualizer, in meters. |
| `label_height` | double | `0.1` | The height of the text labels displayed next to each frame. |
| `position_scale` | double | `1.0` | A scaling factor applied to the position of each frame for visualization purposes. |
| `frame_filter` | list | *(none)* | If set, only the frames explicitly listed are displayed (e.g. `--frame_filter "(left_hand right_hand)"`). When provided, the listed frames are queried directly instead of calling `getAllFrameIds`. The root frame is always displayed. |

### Transform Client Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `tfDevice` | string | `"frameTransformClient"` | The YARP device name for the transform client. |
| `tfFile` | string | `"ftc_yarp_only.xml"` | The XML configuration file for the transform client. |
| `tfRemote` | string | *(none)* | The remote prefix for the transform server. Only set if specified. |

---

## Coordinate Conventions

By default, the utility uses the **OpenXR convention**:
- **-Z** is forward
- **-X** is left
- **+Y** is up

When `z_up` is set, the **standard robotics convention** is used:
- **+X** is forward
- **+Y** is left
- **+Z** is up
