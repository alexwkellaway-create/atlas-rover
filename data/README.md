# Data

Recorded logs, sensor bags, datasets, and saved maps.

**Contents are gitignored.** This folder exists in the repository; what you put in it does not get committed.

## Why it's excluded

Robotics data is enormous — a few minutes of camera and LiDAR recording runs to gigabytes. Git stores every version of every file forever, so a single committed bag file bloats the repository permanently for everyone, on every clone, and is genuinely unpleasant to remove afterwards.

## Where it should actually live

Pick external storage — an external drive, NAS, or object storage — and record *where* in a committed index (`data/INDEX.md` works). Keep for each recording: date, rover configuration, conditions, and what you were testing.

An unlabelled bag file is nearly worthless six months on. You won't remember which run it was.

Small representative samples for automated tests are the exception — those are committed, in [tests/fixtures/](../tests/fixtures/).
