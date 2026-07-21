# Atlas Rover

Building an autonomous robotics platform to learn AI, computer vision, mapping and robotics.

A long-term personal project, structured for a multi-year build.

---

## The goal

**A small ground rover that can be given a destination and reach it on its own** — building a map of somewhere it has never been, seeing what is in front of it, and deciding for itself how to get around it.

Four hard problems in one sentence, and they are the reason the project exists:

| Capability | The question it answers |
|---|---|
| **Navigation** | Where should I go, and how do I get there without hitting anything? |
| **Mapping (SLAM)** | Where am I, and what does this place look like? |
| **Computer vision** | What is in front of me? |
| **AI** | Can I learn to do this better than I was explicitly programmed to? |

The aim is to *understand* autonomous robotics, not to own an autonomous robot. Buying a finished platform would be faster and would teach nothing.

Full reasoning in [docs/project-vision.md](docs/project-vision.md).

---

## Status

**v0.1 — Foundations.** Architecture and documentation established. No robotics code yet.

Next: choose middleware (ROS 2 vs. custom), which constrains everything after it.

| Doc | |
|---|---|
| [docs/project-vision.md](docs/project-vision.md) | Long-term goal, principles, non-goals |
| [docs/roadmap.md](docs/roadmap.md) | Versioned plan, v0.1 → v1.0 |
| [docs/engineering-principles.md](docs/engineering-principles.md) | How the code gets written — operating rules |
| [docs/journal.md](docs/journal.md) | Running engineering log |

---

## Roadmap at a glance

| Version | Capability |
|---|---|
| v0.1 | Foundations — decisions made, project builds |
| v0.2 | A rover exists in simulation and can be driven |
| v0.3 | A physical rover exists and can be driven |
| v0.4 | It can see — camera pipeline and obstacle detection |
| v0.5 | It knows where it is — odometry and localisation |
| v0.6 | It builds maps — SLAM |
| v0.7 | **It drives itself** — autonomous navigation |
| v0.8 | It survives failure — recovery and safety |
| v0.9 | It learns — first trained component |
| v1.0 | It is reliable — sustained untethered autonomy |

One significant capability per version, and every version ends in a rover that works. Details and completion criteria in [docs/roadmap.md](docs/roadmap.md).

---

## Repository layout

```
atlas-rover/
├── docs/          Documentation, architecture, decision records
├── hardware/      Physical build: BOM, wiring, CAD, calibration
├── software/      Everything that runs on the rover
├── simulation/    Virtual rover, worlds, test scenarios
├── tests/         Unit → integration → simulation → hardware
├── scripts/       Setup, build, deploy, data tooling
├── assets/        Images, diagrams, demo media
├── data/          Recordings and datasets (gitignored)
└── models/        Trained weights (gitignored)
```

Every folder has its own README explaining what belongs in it.

---

## The organising principles

Three rules decide where anything goes. Worth stating explicitly, because the value of a structure is that it removes decisions.

**1. `software/` is what runs on the rover.**
Perception, SLAM, and navigation are all software, so they are packages inside it rather than siblings of it. Simulation, docs, and scripts support the work without shipping to the robot, so they sit outside.

**2. Dependencies point downward.**

```
bringup → navigation · ai · teleop → slam · perception → drivers → common
```

`navigation/` may use `perception/`. Never the reverse. This is what keeps subsystems independently testable as the codebase grows.

**3. `drivers/` is the only layer that touches hardware.**
Everything above consumes an interface. That is what lets the identical stack run in simulation and on the physical rover — and it is the difference between one codebase and two that slowly diverge.

---

## Working conventions

**Record decisions.** Anything expensive to reverse — middleware, SLAM backend, control architecture — gets a file in [docs/architecture/decisions/](docs/architecture/decisions/). Ten minutes now; the alternative is reverse-engineering your own reasoning after a six-month gap.

**Keep the journal.** [docs/journal.md](docs/journal.md) is where the learning accumulates. Failures especially — those are the entries worth rereading.

**Never commit large binaries.** Recordings go in `data/`, weights in `models/`, both gitignored. Git keeps every version forever; one committed bag file bloats every clone permanently.

**Simulate first, verify on hardware.** Sim results are evidence. The field is ground truth.

**One capability at a time.** Two half-finished capabilities are worth less than one that works.

---

## Getting started

Nothing to build yet. When there is, setup lives in [scripts/setup/](scripts/setup/) and instructions in [docs/guides/](docs/guides/).
