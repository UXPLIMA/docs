---
title: brix.conf
order: 617
description: "Brix: creative plots, the plot sky and build contests."
icon: blocks
---

Brix: an island that is a creative building plot. Its team builds in creative; nothing hurts, nobody goes hungry, and there is nothing to progress through. Visitors look at what was built.

The file is `plugins/uxmSkyblock/modules/brix.conf`. Every default below is the shipped value.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Whether islands can be made as Brix plots. |

## `ground`

The flat ground a new plot is laid on. The top layer is the one players arrive on. A preset that wants its own terrain names a schematic and leaves "uxm:plot" out of its start list.

| Key | Default | What it does |
|---|---|---|
| `radius` | `32` | How far the ground reaches from the centre, each way. From 0 to 128. |
| `layers` | `GRASS_BLOCK`, `DIRT`, `DIRT`, `DIRT`, `BEDROCK` | The layers of the ground, top first, at most 64. An empty list lays no ground: the plot is void. |

## `modes`

The game modes on a plot. Whatever a player brings to a plot is kept aside while they are on it and given back when they leave: their items, experience, effects and mode. Nothing held on a plot leaves it.

| Key | Default | What it does |
|---|---|---|
| `build` | `CREATIVE` | The mode of the plot's own team while they are on it. |
| `visit` | `ADVENTURE` | The mode of everybody else on the plot. Not creative and not spectator. |
| `keep-permission` | `uxmskyblock.brix.keepmode` | A player with this permission keeps their own mode and items on a plot. Empty for nobody. |

## `rules`

What cannot happen on a plot. Items are never dropped or thrown on a plot and an ender chest does not open there, whatever is written here: that is what keeps creative items on the plot.

| Key | Default | What it does |
|---|---|---|
| `no-damage` | `true` | Nothing hurts a player on a plot, but the void below it. |
| `no-hunger` | `true` | Nobody goes hungry on a plot. |
| `no-spawning` | `true` | No creature comes of itself onto a plot: not out of the dark, not with a patrol or a raid. One a player brings, from an egg, a spawner or a command, or by breeding, comes as it would. |

## `contest`

Build contests. An operator opens one with /isadmin brix startcontest, on a theme named there or one of the list below. While it builds, a plot's team enters it with /is contest join and builds on the theme. While it takes votes, anyone standing on an entered plot not their own gives it stars with /is contest vote. Once the votes end the plots are placed by their stars on average, then by how many voted, and the members of each are paid.

| Key | Default | What it does |
|---|---|---|
| `themes` | `lighthouse`, `castle`, `treehouse`, `village`, `ship`, `garden` | The themes a contest is opened on when the operator names none, one at random. The language files word each one under brix.contest.themes; a theme they do not word is shown as written. |
| `build` | `3d` | How long plots are entered and built. |
| `vote` | `1d` | How long votes are taken once the building ends. |
| `most-stars` | `5` | The most stars a vote gives. From 1 to 10. |
| `min-votes` | `3` | The fewest votes a plot needs to be placed and paid at all, so a plot nobody looked at earns nothing and a plot alone in a contest does not win it by being there. 0 places every plot. |

## `contest.rewards`

What the members of the placed plots are paid once the contest ends, at once when they are online and when they next join otherwise. Only a member who was on the team before the votes began is paid. Each is a list of action lines, as in effects.conf, run for the member in the order written: [console] lines run a command, as "[console] eco give %player_name% 100". %theme% is the contest's theme and %place% the plot's place.

| Key | Default | What it does |
|---|---|---|
| `entered` | empty list | Every placed plot. |
| `places` | `[]`, `[]`, `[]` | The plots placed first, second and on, first place first: one list each. Add or take off places freely; a plot placed past the last list is paid the entered list alone. |
