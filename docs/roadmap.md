# Roadmap

Development divided into versions. **Each version adds exactly one significant capability** and ends in a rover that works.

No dates. On a solo project with no deadline, dates become guilt rather than information — and they are always wrong. Relative size is given instead, which is the part that is actually useful for planning.

> **Size key:** ● small (a few sessions) · ●● medium (several weeks of evenings) · ●●● large (months)

See [project-vision.md](project-vision.md) for where this is all going.

---

## Overview

| Version | Capability | Size |
|---|---|---|
| v0.1 | Foundations — decisions made, project builds | ● |
| v0.2 | A rover exists in simulation and can be driven | ●● |
| v0.3 | A physical rover exists and can be driven | ●●● |
| v0.4 | It can see — camera pipeline and obstacle detection | ●● |
| v0.5 | It knows where it is — odometry and localisation | ●● |
| v0.6 | It builds maps — SLAM | ●●● |
| v0.7 | **It drives itself** — autonomous navigation | ●●● |
| v0.8 | It survives failure — recovery and safety | ●● |
| v0.9 | It learns — first trained component | ●● |
| v1.0 | It is reliable — sustained untethered autonomy | ●● |

---

## v0.1 — Foundations

*Decide the ground rules before writing code that assumes them.*

- [ ] Choose middleware: ROS 2 or custom → decision record
- [ ] Choose language(s) and toolchain
- [ ] Choose simulator
- [ ] Define coordinate frame conventions → `reference/`
- [ ] Build system and CI running

**Done when:** an empty project builds and tests run automatically on commit.

Unglamorous, and the temptation is to skip it. Don't — the middleware choice constrains everything after it, and changing it at v0.5 means rewriting v0.2 through v0.4.

---

## v0.2 — Simulated rover

*A robot you can drive before a robot you can touch.*

- [ ] Rover model with realistic dimensions and mass
- [ ] A world to drive it in
- [ ] Simulated camera, IMU, wheel encoders
- [ ] Teleop control
- [ ] Sensor noise modelling

**Done when:** you can drive it around the simulator with a gamepad.

Model the noise honestly. A noiseless simulator teaches the rover habits that fail immediately on real sensors, and you will not find out until v0.3.

---

## v0.3 — Physical rover

*Bring hardware up to the same baseline as sim.*

- [ ] Finalise BOM, order parts
- [ ] Assemble chassis and drivetrain
- [ ] Wire electronics, document pinouts as you go
- [ ] Motor and sensor drivers behind the **same interfaces as sim**
- [ ] Calibrate camera, IMU, wheel odometry
- [ ] Teleop on hardware

**Done when:** the same teleop code drives both simulator and rover, unchanged.

That condition *is* the deliverable. If it does not hold, the driver abstraction is wrong, and every capability after this gets built twice.

Budget the most calendar time here of anything before v0.6. Parts arrive late, connectors are wrong, and something will be dead on arrival. This is normal.

---

## v0.4 — Vision

*Give it eyes.*

- [ ] Camera capture and image pipeline
- [ ] Obstacle detection
- [ ] Depth estimation (stereo, or a depth camera)
- [ ] Evaluation against labelled fixtures

**Done when:** it reliably identifies obstacles in front of it, in sim and on hardware.

Build the evaluation set before the detector. Without it, "reliably" is a feeling rather than a number, and you cannot tell whether a change helped.

---

## v0.5 — Localisation

*Know where you are.*

- [ ] Wheel odometry
- [ ] IMU integration
- [ ] Sensor fusion for pose estimation
- [ ] Transform tree
- [ ] Quantify drift over a known path

**Done when:** the rover tracks its own position over a few minutes of driving with understood, measured error.

The difficulty steps up here. Wheels slip, IMUs drift, and errors accumulate — the fusion that manages this is genuinely subtle. Expect to spend real time on it. It is also the point where the coordinate frame conventions from v0.1 either pay off or cost you a week.

---

## v0.6 — Mapping (SLAM)

*Know what the place looks like.*

- [ ] Occupancy grid mapping
- [ ] Scan matching
- [ ] Loop closure
- [ ] Map save and reload
- [ ] Validate map against real measurements

**Done when:** it traverses a space and produces a map that matches a tape measure.

The hardest thing in the project, and the one most likely to stall momentum. Two things help: v0.5 must be genuinely solid first, since SLAM inherits every localisation error; and use an existing SLAM library for the first working version, then reimplement to understand it. Understanding a working system beats debugging a broken one you wrote.

---

## v0.7 — Autonomous navigation

*The milestone the whole project points at.*

- [ ] Global path planning
- [ ] Local planning and obstacle avoidance
- [ ] Control loop — plan to motor commands
- [ ] Goal handling

**Done when:** given a goal position, the rover reaches it without collision — in sim, then on hardware.

The first time this works on the physical rover is the moment the project pays for itself. Everything before it was building toward this; everything after makes it dependable.

---

## v0.8 — Robustness

*Handle the world not cooperating.*

- [ ] Recovery behaviours — stuck, lost, blocked
- [ ] Emergency stop, hardware and software
- [ ] Sensor dropout and failure handling
- [ ] Battery monitoring and low-power behaviour
- [ ] Logging good enough to diagnose a failure after the fact

**Done when:** the rover handles being physically stuck, blocked, or losing a sensor — without human intervention.

Less exciting than v0.7 and more important than it looks. The gap between "worked once in a demo" and "works" is almost entirely this version.

---

## v0.9 — Learning

*Deliberately last.*

- [ ] Data collection pipeline via teleop
- [ ] Labelling workflow
- [ ] First learned component — terrain classification is a reasonable start
- [ ] Inference within the rover's compute budget
- [ ] **Benchmark against the v0.4–v0.7 classical baseline**

**Done when:** a trained model runs on the rover and measurably beats the hand-written approach it replaces.

Last on purpose. Classical implementations give you a baseline, and without one you cannot tell whether a model is helping or just adding an opaque failure mode. If the model does not beat the baseline, that is a real and useful result — keep the baseline.

---

## v1.0 — Reliable autonomy

*Make it dependable.*

- [ ] Runs untethered — no laptop attached
- [ ] Full mission on one battery charge
- [ ] Repeatable across multiple unfamiliar spaces
- [ ] Documented setup and operation
- [ ] Demo video

**Done when:** placed in an unfamiliar indoor space and given a goal, it explores, maps, avoids new obstacles, and arrives — repeatedly, unattended, recovering on its own.

Ten successful runs in three different rooms, without intervention. That is v1.0.

---

## After v1.0

Directions, not commitments — outdoor operation, semantic mapping, dynamic obstacles, multi-session maps, manipulation, a second rover. See [project-vision.md](project-vision.md).

---

## Using this document

**Reread at each version boundary.** Expect it to change. A roadmap that survives a multi-year project unedited means nothing was learned along the way.

**Do not skip ahead.** The ordering is dependency, not preference. Attempting SLAM on unreliable odometry produces a debugging problem with no bottom to it.

**Ship each version working.** No version is done while the rover is in pieces. Tag it, write the [journal](journal.md) entry, then start the next.
