# Engineering Journal

Running log of work on Atlas Rover. **Newest entries at the top.**

---

## Why this exists

Git records what changed. This records what you were *thinking* — what broke, what you tried, what you would do differently. That context is genuinely gone otherwise.

It pays off in two places:

**Returning after a gap.** Three months away from a multi-year project is normal. This is what rebuilds your mental model in an evening instead of a weekend.

**Repeating your own mistakes.** You will forget that you already tried a thing and why it failed. This is the only place that is written down.

## How to use it

Write an entry on any day you did real work. Skipping days is fine; skipping the entry for a session that broke something is not.

**Record the failures.** The entries worth rereading are the ones about what did not work. Successes end up documented in the code; dead ends are documented nowhere else, and they are what you are most likely to repeat.

**Write it the same day.** By tomorrow you will have compressed four hours of confusion into "fixed the transform bug", which is exactly the detail that was worth keeping.

**Keep it cheap.** Three honest lines beat a polished paragraph you did not write. A journal with a heavy template becomes a journal that stops getting written.

---

## Entry template

Copy this block for a new entry. Drop any heading with nothing to say.

```markdown
## YYYY-MM-DD — Short title

**Version:** v0.x
**Time:** ~Nh

### Did
What you actually worked on.

### Learned
Anything that surprised you, or that you had to look up and would look up again.

### Broke
What failed, and why. Keep this even after it is fixed — especially then.

### Next
The one thing to pick up next session. Write it specifically enough
that starting tomorrow costs no thinking.
```

### Optional headings

Add when relevant:

- **Decision** — a choice made. If it is expensive to reverse, write an [ADR](architecture/decisions/) too and link it.
- **Measured** — numbers. Drift in cm, loop rate in Hz, detection accuracy. Numbers are what let you tell progress from the feeling of progress.
- **Parts** — hardware ordered, received, or destroyed.
- **Open question** — something unresolved you do not want to lose.

---

## Example entry

*Illustrative — how much detail is worth capturing.*

```markdown
## 2026-09-14 — Odometry drift on carpet

**Version:** v0.5
**Time:** ~3h

### Did
Wheel odometry over a 5 m straight line, ten runs, tile and carpet.

### Measured
Tile: 4–7 cm final error. Carpet: 22–31 cm, consistently short.

### Learned
Carpet compresses under the wheels, so effective radius shrinks and encoder
counts overestimate distance. Not slip — the error is systematic, not random,
which is why it looked like a calibration bug for the first hour.

### Broke
Spent that hour recalibrating the wheel radius, which fixed carpet and broke
tile. Two surfaces cannot share one constant. Reverted.

### Next
Stop treating this as a calibration problem. Fuse IMU acceleration to detect
the mismatch instead — start reading on complementary filters.
```

---

# Entries

*Newest first. Add above the previous entry.*

---

## 2026-07-21 — Repository structure and initial documentation

**Version:** v0.1
**Time:** ~2h

### Did
Set up the repository architecture and wrote the initial docs — vision, roadmap,
this journal. No robotics code, deliberately: wanted the structure settled
before there is anything to move around.

### Decision
`software/` is the source root for everything that runs on the rover, with
`perception/`, `slam/`, and `navigation/` as packages inside it rather than
top-level folders. The flat version had `software/` sitting beside `vision/`
and `navigation/`, which are themselves software — an ambiguity that would
have caused drift once files accumulated.

Set the layering rule at the same time: dependencies point downward only, and
`drivers/` is the sole layer touching hardware. That second rule is what should
let the same stack run in simulation and on the rover.

Named the package `perception/` rather than `vision/` — it will handle LiDAR
and fused input eventually, and packages with real dependents rarely get renamed.

### Learned
Structuring `tests/` by what a test *needs* (nothing / simulator / physical
rover) is more useful than structuring it by subsystem. It answers the question
you actually have, which is what can run right now with no rover on the desk.

### Next
v0.1, first decision: middleware — ROS 2 or custom. Language, simulator, and
build tooling all follow from it, so it goes first. Write it up as ADR-0001.

---

<!-- New entries go above this line -->
