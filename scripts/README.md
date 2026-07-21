# Scripts

Automation for the work around the code — setup, build, deploy, data handling, developer chores.

| Folder | Purpose |
|---|---|
| `setup/` | Environment bootstrap and dependencies |
| `build/` | Compilation and packaging |
| `deploy/` | Getting code onto the rover |
| `data/` | Recording, replaying, converting logs |
| `dev/` | Linting, formatting, local conveniences |

## What belongs here

Scripts here are *about* the project. Code that runs *as part of* the rover belongs in [software/](../software/), even if it's small and script-shaped.

The test: if the rover is driving autonomously in a field, is this executing? If yes, it's software.

## Automate on the second repeat

The rule that pays off over years: the first time you do something by hand, fine. The second time, write it down here.

Deployment especially. A manual deploy process is where you eventually push the wrong branch to the rover — and you'll be doing it outdoors, in the cold, with a laptop balanced on your knee.
