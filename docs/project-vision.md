# Project Vision

*What Atlas Rover is for, and what "finished" would look like.*

---

## The goal

**A small ground rover that can be given a destination and reach it on its own — building a map of somewhere it has never been, seeing what is in front of it, and deciding for itself how to get around it.**

That single sentence contains four hard problems, and they are the reason the project exists:

| Capability | The question it answers |
|---|---|
| **Navigation** | Where should I go, and how do I get there without hitting anything? |
| **Mapping (SLAM)** | Where am I, and what does this place look like? |
| **Computer vision** | What is in front of me? |
| **AI** | Can I learn to do this better than I was explicitly programmed to? |

Each is a serious field on its own. Together they are a rover.

---

## Why build it

To *understand* autonomous robotics, not to have an autonomous robot.

That distinction sets almost every decision downstream. Buying an off-the-shelf autonomous platform would produce a working rover faster and teach nearly nothing. Implementing SLAM badly, watching the map smear when the wheels slip, and working out why — that is the actual deliverable.

Consequences of taking that seriously:

- **Classical methods before learned ones.** A hand-written path planner is debuggable and teaches you what the problem actually is. Reach for a neural network once you have a baseline it has to beat.
- **Build the boring layers yourself.** Odometry, transforms, and coordinate frames are where the real understanding lives.
- **Prefer a working simple thing to a broken sophisticated one.** A rover that reliably crosses a room beats one that theoretically does SLAM but has never left the simulator.

---

## What success looks like

Concrete enough to know when it happens:

**v1.0** — The rover is placed in an unfamiliar indoor space. It is given a goal position. It explores, builds a map, avoids obstacles it has never seen before, and arrives. It does this repeatedly, without a laptop tethered to it, and recovers on its own when something goes wrong.

That is the target. Everything in the [roadmap](roadmap.md) is a step toward it.

---

## Beyond v1.0

Speculative, listed to give direction rather than commitment:

- **Outdoor operation** — rough terrain, GPS fusion, changing light
- **Semantic mapping** — a map that knows *doorway* and *chair*, not just *occupied*
- **Dynamic environments** — people and objects that move while you map
- **Long-horizon autonomy** — multi-session maps, returning to charge
- **Manipulation** — an arm, and everything that follows from it
- **Multi-agent** — a second rover, and coordination between them

---

## Non-goals

Stating these prevents slow scope creep, which is the failure mode that actually kills long personal projects.

- **Not a product.** No users, no support burden, no deadlines.
- **Not a drone or a legged robot.** Wheeled ground vehicle. Flight and legged locomotion are entire projects of their own.
- **Not novel research.** Implementing known algorithms well is the goal. Publishing is not.
- **Not fast.** There is no deadline. Optimising for shipping speed defeats the purpose of building it to learn.

---

## Guiding principles

**Simulation first, hardware always.**
Simulation is where you iterate; the field is where you find out. Anything working in sim is a hypothesis until the rover does it on carpet.

**One capability at a time.**
Each version in the roadmap adds exactly one significant thing. Two half-finished capabilities are worth less than one that works.

**The rover must always be drivable.**
Teleop is maintained from v0.2 onward, forever. It is how you recover a stuck rover, and how you collect training data.

**Write down why.**
Decisions go in [architecture/decisions/](architecture/decisions/), daily work goes in the [journal](journal.md). On a multi-year project, undocumented reasoning is lost reasoning.

**Every version must run.**
No version is complete while the rover is in pieces. Always return to a working state.

---

## The honest risks

Worth naming now, since most long personal robotics projects die of one of these:

- **Scope creep** — the antidote is the non-goals list above, reread periodically.
- **Hardware stalls** — a fried motor controller can idle a project for weeks. Simulation work continues meanwhile, which is a large part of why it comes first.
- **The v0.4 → v0.5 gap** — localisation and SLAM are a genuine step up in difficulty from everything before them. Expect this to be where momentum is most at risk.
- **Long gaps away** — unavoidable in a multi-year project. The journal exists so returning after three months costs an evening rather than a weekend.
