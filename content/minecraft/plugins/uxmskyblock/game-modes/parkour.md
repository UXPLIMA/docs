---
title: Parkour
order: 100
description: An island that is a course its team builds and anybody runs, with checkpoints, best
  times, ghosts, rewards, tags and ratings.
icon: flag
---

A Parkour island is a course. Its team builds the course and anybody runs it. A run starts on a
plate over the start block, is saved at every checkpoint plate, and ends on a plate over the
finish block. The best time of every runner on every course is kept.

## Turning it on

1. Set `enabled = true` in `modules/parkour.conf`.
2. Add `parkour` to `presets.modes` in `modules/presets.conf`.

The shipped preset `parkour` starts with `"uxm:platform"` and `"uxm:course"`, which makes the
island a course with its start plate.

## Marking a course

| Setting | Shipped | What it does |
| --- | --- | --- |
| `markers.plate` | `LIGHT_WEIGHTED_PRESSURE_PLATE` | The plate a run starts and ends on |
| `markers.start-under` | `EMERALD_BLOCK` | The block under a plate that makes it the start |
| `markers.finish-under` | `GOLD_BLOCK` | The block under a plate that makes it the finish |
| `markers.checkpoint` | `HEAVY_WEIGHTED_PRESSURE_PLATE` | The plate that is a checkpoint wherever it stands |

## Runs

| Setting | Shipped | What it does |
| --- | --- | --- |
| `runs.fall-depth` | `8` | How far below the last checkpoint a runner may fall before being put back on it |
| `runs.time-limit` | `30m` | How long a run may take before it is dropped |
| `runs.board-size` | `10` | How many runners `/is course` shows, fastest first |
| `runs.splits` | `true` | Whether each checkpoint says the time to it, ahead of or behind the runner's best |

## Rewards

Each reward is a list of action lines, as in `modules/effects.conf`, run for the runner in the
order written. `%time%` is the run's time, `%millis%` the same in milliseconds, and `%was%` the time
beaten.

| List | When it pays |
| --- | --- |
| `rewards.first-finish` | The first time a runner finishes a course |
| `rewards.personal-best` | Each time a runner beats their own best |
| `rewards.course-record` | Each time a runner finishes faster than anybody has |
| `rewards.season-record` | Each time a runner finishes faster than anybody has in the season under way |

`again-after` is how long before a new best or record on the same course pays the same runner
again. A first finish is paid once for good. A run quicker than `shortest-run` pays nothing, so a
course of a step or two earns no reward. Rewards are never paid to the course's own team.

## Tags and ratings

A course's team gives it one difficulty and one category with `/is coursetag`, by the names of
`tags.difficulties` and `tags.categories`. The language files word each one under
`parkour.difficulties` and `parkour.categories`. Add, take off or rename them freely.

A runner who has finished a course and is not of its team rates it with `/is courserate`, from 1
star to `ratings.stars`. A second rating takes the place of the first.

## The ghost

Every runner's best run is recorded as it goes, and the next run is raced against it: an item only
the runner sees retraces the run from the start plate to the finish.

| Setting | Shipped | What it does |
| --- | --- | --- |
| `ghost.against` | `RECORD` | `RECORD` races the course's fastest run; `OWN` the runner's own best |
| `ghost.sample-every` | `100ms` | How often a run is sampled. At least 50ms |
| `ghost.item` | `PLAYER_HEAD` | What the ghost is drawn as. A head wears the face of whoever ran it |
| `ghost.height`, `.scale`, `.glow` | `1.4`, `0.6`, `true` | How it floats, how large, and whether it shows through the course |

## Fair runs

`checks` ends a run that is not run on foot. The course's own return to the last checkpoint after
a fall is never counted.

- `flight`: flying or opening wings ends a run.
- `teleport`: any teleport ends it, a pearl, chorus fruit, a command or another plugin.
- `speed.fastest`: moving faster than this many blocks a second across the ground ends it. Sprint
  jumping is about 7.
- `bypass-permission`: a holder of this node is not checked.

## Game modes on a course

Whatever a player brings to a course is kept aside while they are on it and given back when they
leave: their items, experience, effects and mode. Nothing the team makes in creative leaves the
course, and a run starts empty handed.

- `modes.build`: the mode of the course's team while not running, `CREATIVE` as shipped.
- `modes.play`: the mode of runners and everybody else, `SURVIVAL` or `ADVENTURE`.
- `modes.keep-permission`: a holder keeps their own mode and items on a course.

## Windows

`menus/parkour-courses.conf` is every course on the server, the most run first, opened with
`/is courses`. A Bedrock player gets it as a form.

## Commands

| Command | What it does |
| --- | --- |
| `/is courses` | Lists every course, the most run first, and takes you to the one you choose |
| `/is course [season [number]]` | Shows the best times on the course you stand on, of all time or of a season |
| `/is coursetag <difficulty\|category> [name]` | Tags the course you stand on, or takes a tag off |
| `/is courserate <stars>` | Rates the course you stand on, once you have finished it |
| `/isadmin parkour resettimes <target> [runner]` | Forgets the runs on a course, of every runner or of one |

After a reset the board starts again and the next finish is a first. The admin verb asks
`uxmskyblock.admin.parkour.resettimes`.
