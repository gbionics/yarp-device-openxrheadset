# OpenXrHeadset Configuration Parameters

This document lists all the configuration parameters parsed by the `OpenXrHeadset` device from the YARP configuration file.

---

## General Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `name` | string | `"OpenXrHeadset"` | The prefix name for all ports opened by the device. A leading `/` is prepended if not present. |
| `rpc_name` | string | `"rpc"` | The name of the RPC port. The full port name is `<prefix>/<rpc_name>`. |
| `vr_period` | double | `0.011` | The period of the main thread loop, in seconds. |
| `joypad_control_server_auto` | bool (flag) | `false` | If set (or set to `true`), the joypad control server is automatically started and restarted when the number of buttons/axes/thumbsticks changes. |
| `use_native_quad_layers` | bool (flag) | `false` | If set (or set to `true`), use native OpenXR quad layers instead of OpenGL-based quad layers. |
| `drawable_area` | double | `1.0` | The fraction of the drawable area used for rendering. Must be in `(0, 1]`. Has no effect if `use_native_quad_layers` is `true`. |
| `stick_as_axis` | bool | `false` | If `true`, thumbsticks are exposed as axes instead of sticks in the joypad interface. |
| `tf_root_frame` | string | `"openxr_origin"` | The name of the root transform frame. |

---

## Eye / Display Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `split_eye_ports` | bool (flag) | `true` | Controls the eye display mode. If not set or set to `true`, uses `STEREO_DUAL_PORT` mode (separate ports for left and right eye). If explicitly set to `false`, uses `STEREO_SINGLE_PORT` mode. Cannot be used together with `mono_eye`. |
| `mono_eye` | bool (flag) | `false` | If set (or set to `true`), uses `MONO` mode (single eye rendering). Cannot be used together with `split_eye_ports`. |
| `inter_camera_distance` | double | `0.07` (`0.0` if mono) | Distance between the left and right virtual cameras, in meters. The absolute value is used. |
| `left_azimuth_offset` | double | `0.0` | Azimuth offset for the left eye image, in radians. |
| `left_elevation_offset` | double | `0.0` | Elevation offset for the left eye image, in radians. |
| `left_image_rotation` | double | `0.0` | Counter-clockwise rotation of the left eye image, in radians. |
| `right_azimuth_offset` | double | `0.0` | Azimuth offset for the right eye image, in radians. |
| `right_elevation_offset` | double | `0.0` | Elevation offset for the right eye image, in radians. |
| `right_image_rotation` | double | `0.0` | Counter-clockwise rotation of the right eye image, in radians. |
| `eye_z_position` | double | `-1.0` | The Z position of the eye quad layers. The value is forced to be negative and at least `0.01` in absolute value. |

---

## OpenXR Interface Settings

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `vr_poses_prediction_in_ms` | double | `0.0` | The prediction time for pose tracking, in milliseconds. |
| `hide_window` | bool (flag) | `true` if `use_native_quad_layers` is set, `false` otherwise | If set (or set to `true`), hides the desktop mirror window. |
| `render_in_play_space` | bool (flag) | `false` | If set (or set to `true`), renders in the play space instead of the view reference space. |
| `no_gaze` | bool (flag) | `false` | If set (or set to `true`), disables gaze tracking. |
| `force_use_gaze` | bool (flag) | `false` | If set (or set to `true`), forces the use of gaze tracking even if the runtime reports that the system does not support eye gaze interaction. Device won't start if both `no_gaze` and this flag are set. |
| `no_expressions` | bool (flag) | `false` | If set (or set to `true`), disables facial expression tracking. |
| `no_hand_tracking` | bool (flag) | `false` | If set (or set to `true`), disables hand tracking. |
| `no_fb_body_tracking` | bool (flag) | `false` | If set (or set to `true`), disables Meta (Facebook) body tracking. |
| `pico_motion_tracker_count` | int | `0` | Number of PICO motion trackers to request at startup. |

---

## Pose Filter Settings

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `head_pose_filter_type` | string | `"jump"` | Filter type for the head pose. Allowed values: `"none"`, `"jump"`. |
| `hands_pose_filter_type` | string | `"jump"` | Filter type for hand poses. Allowed values: `"none"`, `"jump"`. |
| `trackers_pose_filter_type` | string | `"jump"` | Filter type for tracker poses. Allowed values: `"none"`, `"jump"`. |

---

## Pose Validity Check Settings

These parameters control the jump filter behavior used for pose publishing.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `pose_check_max_distance` | double | `0.1` | Maximum allowed distance (in meters) between consecutive poses before considering it a jump. |
| `pose_check_max_angle_rad` | double | `0.5` | Maximum allowed angular distance (in radians) between consecutive poses before considering it a jump. |
| `pose_check_expiration_time` | double | `5.0` | Time (in seconds) after which the last valid data is considered expired. |
| `pose_check_max_convergence_time` | double | `3.0` | Maximum time (in seconds) allowed to converge to a jumped pose. |
| `pose_check_convergence_ratio` | double | `0.05` | Convergence ratio for the jump filter. Clamped to the range `[0, 1]`. |

---

## Transform Client Settings

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `tfDevice` | string | `"frameTransformClient"` | The YARP device name for the transform client. |
| `tfFile` | string | `"ftc_yarp_only.xml"` | The XML configuration file for the transform client. |
| `tfLocal` | string | `"<prefix>/tf"` | The local prefix for the transform client. |
| `tfRemote` | string | *(none)* | The remote prefix for the transform server. Only set if specified. |

---

## Poses Labels (`POSES_LABELS` group)

The `POSES_LABELS` group allows renaming pose frame names. Each entry is a list of two elements: the original name and the modified name.

```ini
[POSES_LABELS]
(original_name modified_name)
(another_original another_modified)
```

---

## GUI Elements (`gui_elements` and `GUI_<i>` groups)

The `gui_elements` parameter specifies the number of GUI overlay elements. For each GUI element `i` (0-indexed), a corresponding `GUI_<i>` group must be defined.

### Top-level

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `gui_elements` | int | `0` | Number of GUI overlay elements to configure. |

### `GUI_<i>` group parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `width` | double | **Yes** | — | Width of the GUI quad layer, in meters. |
| `height` | double | **Yes** | — | Height of the GUI quad layer, in meters. |
| `x` | double | **Yes** | — | X position of the GUI quad layer. |
| `y` | double | **Yes** | — | Y position of the GUI quad layer. |
| `z` | double | **Yes** | — | Z position of the GUI quad layer. Forced to be negative and at least `0.01` in absolute value. |
| `visibility` | string | No | `"both"` | Which eye(s) the layer is visible to. Allowed values: `"left"`, `"right"`, `"both"`, `"none"`. |
| `port_id` | string | No | `"gui_<i>"` (lowercase group name) | The port name suffix for receiving images. Full port name: `<prefix>/<port_id>`. |
| `follow_eyes` | bool (flag) | No | `false` | If set (or set to `true`), the GUI element follows the eye gaze direction. |

---

## Label Elements (`labels` and `LABEL_<i>` groups)

The `labels` parameter specifies the number of text label elements. For each label `i` (0-indexed), a corresponding `LABEL_<i>` group must be defined.

### Top-level

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `labels` | int | `0` | Number of label elements to configure. |

### `LABEL_<i>` group parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `width` | double | **Yes** | — | Width of the label quad layer, in meters. |
| `height` | double | **Yes** | — | Height of the label quad layer, in meters. |
| `x` | double | **Yes** | — | X position of the label quad layer. |
| `y` | double | **Yes** | — | Y position of the label quad layer. |
| `z` | double | **Yes** | — | Z position of the label quad layer. Forced to be negative and at least `0.01` in absolute value. |
| `port_id` | string | No | `"label_<i>"` (lowercase group name) | The port name suffix. Full port name: `<prefix>/<port_id>`. |
| `prefix` | string | No | `""` | Text prefix prepended to the received label string. |
| `suffix` | string | No | `""` | Text suffix appended to the received label string. |
| `font` | string | No | `"Roboto/Roboto-Black.ttf"` | Path to the TTF font file used for rendering the label. |
| `pixel_size` | int | No | `64` | Font pixel size. |
| `automatically_enabled` | bool | No | `true` | If `true`, the label is enabled automatically on startup. |
| `disable_timeout_in_S` | double | No | `-1.0` | Timeout (in seconds) after which the label is disabled if no new data is received. A negative value disables the timeout. |
| `follow_eyes` | bool (flag) | No | `false` | If set (or set to `true`), the label follows the eye gaze direction. |
| `horizontal_alignement` | string | No | `"center"` | Horizontal text alignment. Allowed values: `"left"`, `"right"`, `"center"`. |
| `vertical_alignement` | string | No | `"center"` | Vertical text alignment. Allowed values: `"top"`, `"bottom"`, `"center"`. |
| `color` | list of 4 doubles | No | `(1.0 1.0 1.0 1.0)` | Label text color as RGBA values, each in range `[0, 1]`. |
| `background_color` | list of 4 doubles | No | `(0.0 0.0 0.0 0.0)` | Label background color as RGBA values, each in range `[0, 1]`. |

---

## Slide Elements (`slides` and `SLIDE_<i>` groups)

The `slides` parameter specifies the number of slide (image) elements. For each slide `i` (0-indexed), a corresponding `SLIDE_<i>` group must be defined.

### Top-level

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `slides` | int | `0` | Number of slide elements to configure. |

### `SLIDE_<i>` group parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `width` | double | **Yes** | — | Width of the slide quad layer, in meters. |
| `height` | double | **Yes** | — | Height of the slide quad layer, in meters. |
| `x` | double | **Yes** | — | X position of the slide quad layer. |
| `y` | double | **Yes** | — | Y position of the slide quad layer. |
| `z` | double | **Yes** | — | Z position of the slide quad layer. Forced to be negative and at least `0.01` in absolute value. |
| `port_id` | string | No | `"slide_<i>"` (lowercase group name) | The port name suffix. Full port name: `<prefix>/<port_id>`. |
| `slides_path` | string | No | `"./"` | Path to the directory containing the slide images. |
| `initial_slide` | string | No | `""` | Name of the initial slide image to display. Empty string means no initial image. |

---

## Custom Poses (`custom_poses` and `CUSTOM_POSE_<i>` groups)

The `custom_poses` parameter specifies the number of custom pose definitions. For each custom pose `i` (0-indexed), a corresponding `CUSTOM_POSE_<i>` group must be defined.

### Top-level

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `custom_poses` | int | `0` | Number of custom pose elements to configure. |

### `CUSTOM_POSE_<i>` group parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `name` | string | **Yes** | — | The name of the custom pose frame. |
| `parent_frame` | string | **Yes** | — | The parent frame to which this custom pose is relative. |
| `euler_angles` | string | **Yes** | — | A 3-character string specifying the Euler angles parametrization (e.g., `"YXZ"`). Each character must be `x`, `y`, or `z` (case-insensitive). |
| `relative_position` | list of 3 values | **Yes** | — | Relative position offset `(x y z)`. Each element is either a float value or `"*"` (wildcard, meaning the position on that axis is taken from the parent). |
| `relative_rotation` | list of 3 values | **Yes** | — | Relative rotation offset as Euler angles. Each element is either a float value (in radians) or `"*"` (wildcard, meaning the rotation on that axis is taken from the parent). |
| `static_pose` | bool (flag) | No | `false` | If set (or set to `true`), the pose is static. When `true`, all `relative_position` and `relative_rotation` entries must be numeric (no wildcards). |

> **Note on masks:** In `relative_position` and `relative_rotation`, a float value sets a fixed offset, while `"*"` inherits the corresponding component from the tracked parent frame.
