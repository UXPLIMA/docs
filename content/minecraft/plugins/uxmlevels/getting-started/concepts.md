---
title: Concepts
order: 203
description: Tracks, experience and levels, events, rewards, prestige, multipliers, the heads-up surfaces and boards.
icon: lightbulb
---

## Tracks

A track is one ladder of levels, a file in `tracks/` whose name is its id: `tracks/mining.conf` is the track
`mining`. A player climbs every track on its own; mining does not move combat. The shipped tracks are `global`,
`mining` and `combat`, a hundred levels each. See [Tracks](../tracks/tracks.md).

## Experience and levels

A player holds an amount of experience on each track, starting at level 1. The track's curve says what each level
costs: level 1 to 2 costs `base-cost`, and each level after costs the one before times `growth`. Only experience is
stored; the level is worked out from it, so changing a curve changes every player's level at once.

This experience is the plugin's own, apart from vanilla experience. A track can choose to show itself on the vanilla
experience bar; see below.

## Events

An event is a rule in `events/` that pays experience into one track: what happened (`mining`, `combat`, `playtime`,
`fishing`, `eating`, `crafting`, `chat`), which blocks, mobs or items count, how much, how likely and how often.
**Every** event that matches pays, so breaking diamond ore pays both `mining-any` and `mining-ores`. See
[Events](../tracks/events.md).

## Rewards

A track can carry rewards at chosen levels: action lines such as `[console] give {player} diamond 8`. A **manual**
reward is claimed in the rewards window; an **automatic** one runs the moment the level is reached. Each is taken
once per climb. See [Rewards](../tracks/rewards.md).

## Prestige

A player at `prestige.required-level` on a track may prestige it: the track starts again at level 1, its rewards can
be taken again, and every gain on that track is worth `bonus-per-prestige` more, for good, up to `prestige.maximum`
times. See [Prestige](../tracks/prestige.md).

## Multipliers

An earned amount is multiplied by four numbers: the global multiplier, the best permission multiplier the player
holds, a timed boost an operator started, and the prestige multiplier of that track. Experience given by command is
not multiplied. See [Multipliers and boosts](../tracks/multipliers-and-boosts.md).

## The heads-up surfaces

Three surfaces can show a track: the vanilla experience bar, a boss bar, and a line over the hotbar. Each surface
shows one track, the first file that asks for it. As shipped, `global` takes the experience bar, `mining` the boss bar
and `combat` the action bar. See [Heads-up](../tracks/heads-up.md).

## Boards

Every track is a board, kept for all time, the month, the week and the day. All time ranks by level; the other
periods rank by experience earned in them. See [Boards](../tracks/boards.md).
