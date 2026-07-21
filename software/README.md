# Software

Everything that runs on the rover.

This is the source root. If it executes on the robot, it lives here; if it doesn't, it's a sibling of this folder.

## Packages

| Package | Responsibility |
|---|---|
| `common/` | Shared types, transforms, logging, config |
| `drivers/` | Hardware abstraction — motors, IMU, cameras, LiDAR |
| `perception/` | Making sense of sensor data |
| `slam/` | Localisation and mapping |
| `navigation/` | Planning and control |
| `ai/` | Learned models and policies |
| `teleop/` | Manual and remote control |
| `bringup/` | Launch files and runtime parameters |

## The layering rule

Dependencies point downward, never up:

```
bringup
   ↓
teleop · navigation · ai
   ↓
slam · perception
   ↓
drivers
   ↓
common
```

`navigation/` may import from `perception/`. `perception/` must never import from `navigation/`. When you find yourself wanting to violate this, the thing you actually want usually belongs in `common/` — or the two packages are drawn along the wrong boundary.

## Why the abstraction at `drivers/`

`drivers/` is the only layer that touches physical devices. Everything above it consumes an interface.

That single boundary is what makes simulation useful: swap the driver implementation and the entire stack above runs unchanged against the simulator. Without it, sim and hardware quietly become two codebases.

## On `perception/` rather than `vision/`

Named for the job, not the sensor. Cameras come first, but this package will eventually handle LiDAR and fused multi-sensor input, and renaming a package with real dependents is a chore nobody does.
