---
title: Brix
order: 310
description: A creative building plot, with the plot sky and build contests.
icon: blocks
---

A Brix island is a creative building plot. Its team builds in creative; nothing hurts, nobody goes hungry, and there
is nothing to progress through. Visitors look at what was built.

What a player carries is kept aside while they stand on a plot and handed back when they leave, so nothing made in
creative leaves the plot. `uxmskyblock.brix.keepmode` keeps a player's own game mode on a plot.

## The plot

| Command | What it does |
|---|---|
| `/is plot time <setting>` | Sets the time on the plot you stand on |
| `/is plot weather <setting>` | Sets the weather on it |
| `/is plot reset [confirm]` | Clears everything built on it and lays its ground again; the owner only, within 30 seconds |
| `/is plots` | Opens [every plot](../menus/game-modes.md#brix-plots) |

## Build contests

An operator opens a contest with `/isadmin brix startcontest [theme]`. While it builds, a team enters its plot with
`/is contest join`. When the building ends the votes open: players stand on a plot not their own and give it stars
with `/is contest vote <stars>`. When the votes end, the places are announced and paid. A plot needs the fewest votes
set in the file to be placed.

The themes, how long building and voting last, the stars, the rewards per place and the plot's ground are in
[brix.conf](../config/brix-conf.md).
