# Simulation

The virtual rover and the worlds it drives in.

Separate from `software/` on purpose: this doesn't ship to the robot. It's development and test infrastructure.

## Contents

| Folder | Purpose |
|---|---|
| `worlds/` | Environments — terrain, lighting, obstacles |
| `models/` | Robot and object descriptions (URDF/SDF) |
| `scenarios/` | Named, repeatable test situations |
| `config/` | Physics and sensor-noise parameters |

## Why this matters more than it looks

Simulation is what lets you iterate at all. Real-robot test cycles are slow, and a bad autonomy bug on hardware breaks parts.

Two things keep a sim honest:

**Model noise deliberately.** A noiseless simulator teaches the rover habits that collapse on contact with real sensors. `config/` is where you make the sim appropriately unreliable.

**Keep the model synced with hardware.** When `hardware/mechanical/` changes, `models/` must follow. A drifted robot model produces confident test results that mean nothing — arguably worse than no test at all.

Simulation passing is evidence, not proof. Field results are the ground truth.
