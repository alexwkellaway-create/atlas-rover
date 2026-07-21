# Hardware

The physical rover: what it's made of, how it's wired, and how it's been measured.

No source code lives here. Code that *talks* to hardware belongs in [software/drivers/](../software/drivers/).

## Contents

| Folder | Purpose |
|---|---|
| `bom/` | Parts, suppliers, part numbers, cost |
| `electronics/` | Wiring, schematics, pinouts, power budget |
| `mechanical/` | CAD, chassis, drivetrain geometry |
| `datasheets/` | Vendor PDFs, kept locally because links rot |
| `calibration/` | Measured per-unit values for a specific rover |

## Design vs. instance

`bom/`, `electronics/`, and `mechanical/` describe the rover *design*. `calibration/` describes **one physical rover** — its camera intrinsics, its IMU bias, its wheel scale factor.

Keeping that separation means a second build doesn't overwrite the first one's numbers, and it forces the useful question: is this bug in the design or in this unit?
