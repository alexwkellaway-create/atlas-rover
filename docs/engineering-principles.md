# Engineering Principles

Standards for how Atlas Rover is built. Written before the first line of robotics code, while they are still cheap to adopt.

These are **operating rules, not aspirations.** Each one names a specific decision it changes. A principle that cannot tell you what to do on a Tuesday afternoon is decoration, and has been left out.

---

## Architecture

### 1. Interfaces are the contract; implementations are disposable

Modules depend on interfaces, never on each other's internals. An implementation can be rewritten, replaced, or thrown away without anything above it noticing.

This is the single highest-value rule in the project, because it is what makes the same code run in simulation and on hardware.

**In practice:** `navigation/` asks for a velocity command interface. It never learns whether that resolves to a simulated differential drive or a real motor controller over serial. When the motor controller is replaced — and it will be — one file in `drivers/` changes and nothing else does.

---

### 2. Dependencies point downward, always

```
bringup → navigation · ai · teleop → slam · perception → drivers → common
```

Upward or sideways imports are not allowed. Not discouraged — not allowed.

Circular dependencies are the specific failure that makes a robotics codebase untestable, because nothing can be instantiated in isolation and every test drags in the whole stack.

**In practice:** `perception/` wants to know the current goal so it can prioritise obstacles ahead of the rover. That would import `navigation/` and invert the arrow. Instead, `navigation/` passes a region of interest *into* the perception call. Same behaviour, arrow intact, and `perception/` remains testable with nothing but a recorded frame.

---

### 3. Prefer boring technology

Choose the well-documented, widely-used option over the interesting one. Novelty is a cost paid in debugging, and this project's difficulty budget belongs to the robotics, not the tooling.

**In practice:** v0.6 uses an existing, established SLAM implementation to reach a working map — then reimplements it to understand it. Writing SLAM from scratch on top of untested odometry means debugging two unknowns at once, which is how a version stalls for a year.

---

### 4. Configuration is data, not code

Anything you might tune belongs in a config file: gains, thresholds, frame names, topic names, timeouts, sensor rates. Not constants in a source file.

Tuning happens in the field with cold hands and no compiler. Recompiling to change a PID gain turns a five-minute experiment into an afternoon.

**In practice:** a control gain lives in `software/bringup/config/`, not at the top of a controller module. When it needs different values on carpet and on tile, that becomes two config files rather than a code change — and both are recorded in git, so you can see exactly what the rover was running on the day it worked.

---

## Simplicity

### 5. The simplest thing that works, then measure before optimising

Write the obvious implementation first. Optimise only when a measurement — not intuition — shows it matters.

Robotics has real-time constraints, which makes this rule harder to follow and more important. The instinct to hand-optimise perception code before knowing where the time goes is strong and usually wrong.

**In practice:** the vision pipeline in v0.4 misses its frame rate target. Before rewriting anything in C++, profile it. If 80% of the time is in a colour conversion happening twice, deleting the duplicate call is a one-line fix that a rewrite would have preserved.

---

### 6. Delete code rather than commenting it out

Commented-out blocks and dead branches are noise that outlives their author's memory of why they exist. Git remembers everything; the working tree does not need to.

**In practice:** an abandoned obstacle detector is deleted, not left behind an `if False:`. The journal entry records why it was abandoned and the commit hash where it lives, which is more useful than the code itself.

---

## Testing

### 7. Test at the cheapest level that catches the bug

A bug catchable by a unit test should have a unit test, not a simulation run. Cost here means wall-clock time and setup — the expensive tests are the ones you eventually stop running.

**In practice:** a transform-composition error is a unit test with two hardcoded poses that runs in a millisecond. Discovering the same bug by watching the rover drive into a wall costs an hour of setup and tells you far less about the cause.

---

### 8. Every bug becomes a test

When you fix a bug, the fix ships with a test that fails without it. No exceptions, including for bugs found on hardware.

This is what stops a multi-year project from cycling through the same three failures forever, and it is the discipline most likely to be skipped at 11pm when the fix already works.

**In practice:** the rover drops its map on a specific loop closure. Before fixing it, capture the sensor sequence that triggers it into `tests/fixtures/` and write the failing test. The fix is then provably a fix, and the bug cannot silently return in v0.8.

---

### 9. Simulation is a hypothesis; hardware is the result

Passing in simulation means the code is *ready to be tested*, not that it works. No capability is complete until it has run on the physical rover.

Simulators model what their authors thought to model. Real wheels slip, real sensors drop frames, real batteries sag under load — and the gap between the two is where most of the interesting failures live.

**In practice:** v0.7 autonomous navigation passing every simulated scenario does not close v0.7. The version closes when the rover crosses a real room. If the two disagree, the simulator is wrong and `simulation/config/` gets a fix — that disagreement is information, not an annoyance.

---

### 10. Claims need numbers

"Better", "more accurate", and "faster" are not results. Record the measurement, the units, and the conditions.

**In practice:** "improved odometry" is not a journal entry. "Drift over 5 m: 22 cm → 6 cm on carpet, ten runs" is — and it is the only version that lets you tell six months later whether v0.9's learned approach actually beat the classical baseline it replaced.

---

## Safety and observability

### 11. Fail safe, and fail loudly

On any unexpected condition, the rover stops moving and says why. Never continue on a guess.

A robot that degrades quietly is a robot that drives down the stairs. Motion is the dangerous default, so absence of information must mean stop.

**In practice:** the depth camera stops publishing. The navigation stack does not proceed on its last known frame — it halts, logs the specific timeout that triggered it, and requires explicit intervention. Stale sensor data is more dangerous than none, because it looks valid.

---

### 12. Log for the failure you will not be there to see

Field failures are rarely reproducible. Logs are frequently the only evidence you get, so log the inputs and decisions that would let you reconstruct what happened.

**In practice:** log the chosen path and the reason it was chosen, not just the final motor commands. When the rover takes an inexplicable turn in the corner of a room you were not watching, the decision trace is what explains it — the commands only tell you what it did, not why.

---

## Documentation and change

### 13. Decisions get written down

Any choice that is expensive to reverse gets an [ADR](architecture/decisions/): what was chosen, what was rejected, and why.

The rejected alternatives are the valuable part. In a year the question is never "what did I pick" — the code says that. It is "did I already consider X?"

**In practice:** choosing ROS 2 over a custom middleware produces `0001-middleware-choice.md` before the first package is created. When ROS 2 feels heavy at v0.5, that file tells you whether the friction you are hitting is one you already accepted knowingly.

---

### 14. Documentation lives next to what it describes

Docs sit as close to their subject as possible — a README in the folder, a comment on the non-obvious line. Distance is what makes documentation rot, because a doc three directories away never gets updated alongside the code.

**In practice:** the calibration procedure lives in `hardware/calibration/`, not in a general setup guide. Someone changing the camera mount is already in that folder and will see it. They will never think to check `docs/guides/`.

---

### 15. Tag what works before you change it

Before a significant refactor or a field trial, tag the commit. A known-good state you can return to in one command is worth the two seconds it costs.

**In practice:** `v0.5-field-ok` is tagged the evening localisation first works reliably. When a v0.6 SLAM experiment destabilises the pose estimate two weeks later, the question "was this ever actually working?" has a definitive answer instead of an afternoon of bisecting.

---

### 16. Experiments are timeboxed and disposable

Exploratory work happens on a branch with a decided time limit and a question it is meant to answer. At the limit, it merges or it is deleted — and either way the journal records what was learned.

Indefinite experimental branches become a second codebase nobody maintains and nobody quite abandons.

**In practice:** "try a learned local planner" gets a branch and two weekends. If it beats the classical planner on the benchmark, it merges. If not, the branch is deleted and the journal records the numbers and why it lost. Both outcomes are progress; only the undecided third one is waste.

---

## Breaking these

These are defaults, not laws. Any of them can be broken with a reason — but the reason goes in the [journal](journal.md), and if it recurs, the principle itself was wrong and should be amended here.

A rule quietly ignored three times is worse than no rule, because it teaches you that the rest are optional too.

**Reviewed at each version boundary.** Principles that stopped matching how the project actually works get rewritten, not preserved out of politeness.
