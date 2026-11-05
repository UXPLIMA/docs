---
title: parkour.conf
order: 634
description: "Parkour: courses, runs, checks, ghosts, ratings and seasons."
icon: footprints
---

Parkour: an island that is a course. Its team builds the course; anybody runs it. A run starts on a plate over the start block, is saved at every checkpoint plate, and ends on a plate over the finish block. The best time of every runner on every course is kept.

The file is `plugins/uxmSkyblock/modules/parkour.conf`. Every default below is the shipped value.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Whether islands can be made as Parkour courses. |

## `markers`

What marks a course.

| Key | Default | What it does |
|---|---|---|
| `plate` | `LIGHT_WEIGHTED_PRESSURE_PLATE` | The plate a run starts and ends on. |
| `start-under` | `EMERALD_BLOCK` | The block under a plate that makes it the start. |
| `finish-under` | `GOLD_BLOCK` | The block under a plate that makes it the finish. |
| `checkpoint` | `HEAVY_WEIGHTED_PRESSURE_PLATE` | The plate that is a checkpoint wherever it stands. A runner who falls is put back on the last one. |
| `runs.fall-depth` | `8` | How far below the last checkpoint, or the start, a runner may fall before being put back on it. |
| `runs.time-limit` | `30m` | How long a run may take before it is dropped. |
| `runs.board-size` | `10` | How many runners the course command shows, fastest first. From 1 to 100. |
| `runs.splits` | `true` | Whether each checkpoint says how long the run took to it, and how far ahead of or behind the runner's best run at the same checkpoint. |

## `rewards`

What a finish pays. Each is a list of action lines, as in effects.conf, run for the runner in the order written: [console] lines run a command, as "[console] eco give %player_name% 100". %time% is the run's time, %millis% the same in milliseconds, and %was% the time beaten, empty when there was none.

| Key | Default | What it does |
|---|---|---|
| `first-finish` | empty list | The first time a runner finishes a course. |
| `personal-best` | empty list | Each time a runner beats their own best on a course. |
| `course-record` | empty list | Each time a runner finishes a course faster than anybody has, the first finish on a course included. |
| `season-record` | empty list | Each time a runner finishes a course faster than anybody has in the season under way, the season's first finish there included. Seasons come from the season system; with none under way this pays nothing. |
| `again-after` | `24h` | How long after a runner is paid a new best or a record on a course the same list pays them there again. A first finish is paid once for good. Rewards are never paid to the course's own team, who build it. |
| `shortest-run` | `5s` | A run quicker than this pays nothing, so a course of a step or two earns no reward. |

## `tags`

The tags a course's team gives it with the coursetag command: one difficulty and one category, each by a name below. The language files word each one under parkour.difficulties and parkour.categories; a name they do not word is shown as written. Add, take off or rename them freely.

| Key | Default | What it does |
|---|---|---|
| `difficulties` | `easy`, `medium`, `hard`, `expert` | Easiest first. |
| `categories` | `classic`, `speed`, `maze`, `dropper`, `puzzle` |  |

## `ratings`

How runners rate a course with the courserate command: a runner who has finished a course, and is not of its team, gives it from 1 star to the most below, from 2 to 10. A second rating takes the place of the first.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` |  |
| `stars` | `5` |  |

## `ghost`

The ghost a runner races. Every runner's best run on a course is recorded as it goes, and the next run is raced against it: an item only the runner sees retraces the run, from the start plate to the finish.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` |  |
| `against` | `RECORD` | Whose ghost is raced: RECORD for the course's fastest run, whoever ran it, or OWN for the runner's own best, and the course's fastest before they have one. |
| `sample-every` | `100ms` | How often a run is sampled. The ghost moves on a straight line between two samples. At least 50ms. |
| `item` | `PLAYER_HEAD` | The item the ghost is drawn as. A player head wears the face of whoever ran it. |
| `height` | `1.4` | How far above the ground the item floats, how large it is drawn, from above 0 up to 4, and whether it glows, so it shows through the course. |
| `scale` | `0.6` |  |
| `glow` | `true` |  |

## `checks`

What ends a run that is not run on foot. The course's own return to the last checkpoint after a fall is never counted.

| Key | Default | What it does |
|---|---|---|
| `flight` | `true` | Flying or opening wings during a run ends it. |
| `teleport` | `true` | Any teleport during a run ends it: a pearl, chorus fruit, a command or another plugin. |
| `speed.enabled` | `true` | Moving faster than this across the ground ends a run. |
| `speed.fastest` | `16` | The most blocks a second a runner covers, measured over a quarter of a second at least, so a fall does not count. Sprint jumping is about 7, and a Speed effect or ice adds to it. |
| `bypass-permission` | `uxmskyblock.parkour.checks.bypass` | A player with this permission is not checked. Leave it empty for nobody. |

## `modes`

The game modes on a course, which is how a run is played. Whatever a player brings to a course is kept aside while they are on it and given back when they leave: their items, experience, effects and mode. Nothing the team makes in creative leaves the course, and a run starts empty handed.

| Key | Default | What it does |
|---|---|---|
| `build` | `CREATIVE` | The mode of the course's own team while they are on it and not running: how the course is built. |
| `play` | `SURVIVAL` | The mode of anybody running, and of everybody else on the course: SURVIVAL or ADVENTURE. |
| `keep-permission` | `uxmskyblock.parkour.keepmode` | A player with this permission keeps their own mode and items on a course. Leave it empty for nobody. |
