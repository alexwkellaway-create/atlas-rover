# Models

Trained model weights and exported inference artifacts.

**Contents are gitignored** — same reasoning as [data/](../data/): weights are large binaries, and git keeps every version forever.

## Not to be confused with

- [software/ai/](../software/ai/) — the *code* that trains and runs models
- [simulation/models/](../simulation/models/) — robot and object *descriptions* for the simulator

Three different things called "models". Unavoidable — the terms are standard in their own domains — so the separation is kept sharp here.

## Track provenance

For every weight file, record: training dataset, commit hash of the training code, hyperparameters, and validation results. Store it in a committed `MODELS.md` alongside wherever the weights actually live.

A weights file with no provenance is unreproducible. When it starts underperforming in the field you'll have no way to tell what changed — and no way to rebuild it.
