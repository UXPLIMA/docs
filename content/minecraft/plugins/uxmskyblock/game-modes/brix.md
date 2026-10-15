---
title: Brix
order: 110
description: An island that is a creative building plot, safe and calm, with time and weather of
  its own and build contests with votes and rewards.
icon: brick-wall
---

A Brix island is a creative building plot. Its team builds in creative. Nothing hurts, nobody
goes hungry, and there is nothing to progress through. Visitors look at what was built.

## Turning it on

1. Set `enabled = true` in `modules/brix.conf`.
2. Add `brix` to `presets.modes` in `modules/presets.conf`.

The shipped preset `brix` starts with `"uxm:plot"`, the flat ground the team builds on. A plot has
no Nether and no End. A preset that wants its own terrain names a schematic and leaves `"uxm:plot"`
out of its start list.

## The ground

| Setting | Shipped | What it does |
| --- | --- | --- |
| `ground.radius` | `32` | How far the ground reaches from the centre, each way, from 0 to 128 |
| `ground.layers` | grass, three dirt, bedrock | The layers, top first, at most 64. An empty list leaves the plot void |

## Game modes on a plot

Whatever a player brings to a plot is kept aside while they are on it and given back when they
leave: their items, experience, effects and mode. Nothing held on a plot leaves it.

- `modes.build`: the mode of the plot's own team, `CREATIVE` as shipped.
- `modes.visit`: the mode of everybody else, `ADVENTURE` as shipped. Not creative and not
  spectator.
- `modes.keep-permission`: a holder keeps their own mode and items on a plot.

## Rules

Items are never dropped or thrown on a plot and an ender chest does not open there, whatever is
written here: that is what keeps creative items on the plot.

| Setting | Shipped | What it does |
| --- | --- | --- |
| `rules.no-damage` | `true` | Nothing hurts a player on a plot but the void below it |
| `rules.no-hunger` | `true` | Nobody goes hungry on a plot |
| `rules.no-spawning` | `true` | No creature comes of itself onto a plot. One a player brings comes as it would |

## Build contests

An operator opens a contest with `/isadmin brix startcontest`, on a theme named there or one drawn
from `contest.themes`. While it builds, a plot's team enters with `/is contest join` and builds on
the theme. While it takes votes, anyone standing on an entered plot not their own gives it stars
with `/is contest vote`. Once the votes end, the plots are placed by their stars on average, then
by how many voted, and the members of each are paid.

| Setting | Shipped | What it does |
| --- | --- | --- |
| `contest.themes` | six themes | The themes drawn when the operator names none. Worded under `brix.contest.themes` |
| `contest.build` | `3d` | How long plots are entered and built |
| `contest.vote` | `1d` | How long votes are taken once the building ends |
| `contest.most-stars` | `5` | The most stars a vote gives, from 1 to 10 |
| `contest.min-votes` | `3` | The fewest votes a plot needs to be placed and paid at all |

`contest.rewards.entered` pays every placed plot, and `contest.rewards.places` holds one list per
place, first place first. A plot placed past the last list is paid the entered list alone. Each is
a list of action lines, as in `modules/effects.conf`, with `%theme%` and `%place%`. Only a member
who was on the team before the votes began is paid, at once when online and when they next join
otherwise.

## Commands

| Command | What it does |
| --- | --- |
| `/is plots` | Lists every plot, those in the contest first, and takes you to the one you choose |
| `/is plot <time\|weather> <setting>` | Sets the time or weather of the plot you stand on |
| `/is plot reset [confirm]` | Clears everything built on the plot and lays its ground again |
| `/is contest` | Shows the contest under way |
| `/is contest join` | Enters the plot you stand on |
| `/is contest vote <stars>` | Gives the plot you stand on your stars |
| `/isadmin brix startcontest [theme]` | Opens a contest |
| `/isadmin brix stopcontest` | Takes the contest under way off, with its entries and votes |

Each admin verb asks `uxmskyblock.admin.brix.<verb>`.
