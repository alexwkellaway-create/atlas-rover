# Tests

Organised by **what a test needs to run**, not by which package it covers. That's the property that decides when a test can run, and it's what makes the split useful.

| Folder | Needs | Speed | Runs |
|---|---|---|---|
| `unit/` | Nothing | Milliseconds | Every commit |
| `integration/` | Multiple packages | Seconds | Every commit |
| `simulation/` | The simulator | Minutes | Before merge |
| `hardware/` | The physical rover | Manual | Before a field trial |
| `fixtures/` | — | — | Shared test data |

## Why this axis

Grouping by subsystem (`tests/navigation/`, `tests/perception/`) reads well but answers the wrong question. What you need at 11pm is "what can I run right now, on a laptop, with no rover on the desk?" — and that's what this layout tells you at a glance.

## Hardware tests are records, not just scripts

`tests/hardware/` is partly procedural — a human drives the rover and observes. Record every run with the date, the firmware version, and the conditions. Over a multi-year project these accumulate into the only real evidence of whether the rover is getting better, and memory is not a substitute.

## Keep fixtures small

Committed test data stays tiny — a few frames, one short scan. Full recordings and datasets go in [data/](../data/), which is gitignored. A repository that swallowed a 4 GB bag file is painful to undo.
