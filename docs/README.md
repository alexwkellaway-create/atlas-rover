# Documentation

Everything written for humans rather than machines.

## Start here

| Document | Purpose |
|---|---|
| [project-vision.md](project-vision.md) | The long-term goal, principles, and non-goals |
| [roadmap.md](roadmap.md) | Versioned plan from v0.1 to v1.0 |
| [engineering-principles.md](engineering-principles.md) | How the code gets written — operating rules |
| [journal.md](journal.md) | Running engineering log — newest first |

These four live at the top of `docs/` rather than in a subfolder because they are the ones you reread; everything below is looked up as needed.

Docs are split by *what you're doing when you reach for them*, which is the split that survives as a project grows:

| Folder | You're here because… |
|---|---|
| `architecture/` | You want to understand how the system fits together |
| `architecture/decisions/` | You want to know *why* it's built that way |
| `guides/` | You're trying to accomplish a specific task |
| `reference/` | You need to look up a specific fact |
| `research/` | You're evaluating an approach you haven't committed to |

## The one habit worth keeping

Write a decision record whenever you make a choice that would be expensive to reverse — middleware, SLAM backend, control architecture. It takes ten minutes and it's the difference between a project you can return to after six months away and one you have to reverse-engineer.
