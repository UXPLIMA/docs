---
title: Concepts
order: 103
description: The grid, plot ids, owners and grades, visit policies, and things worked out from the clock.
icon: lightbulb
---

## The grid

A plot world is a grid of square plots with roads between them, written into the chunks by the plot generator. With
plots of 64 and roads of 7, plot `0;0` covers blocks 0 to 63 and plot `1;0` starts at 71. The grid, layers, walls, road
and biome cannot change once the world exists: existing plots would no longer match, so the change is refused.

## Plot ids

A plot is written `world:x;z`, such as `creative:-3;5`. `/plot tp creative:-3;5` goes there; `/plot info` names the
one you stand on.

## Owner, members, visitors

One owner. Members hold any of four grades: `build`, `interact`, `container`, `settings`. A visitor may come in when
the plot's visit policy allows: `open`, `trusted` or `closed`. A banned player may not.

## Everything from the clock

A contest's phases, a plot's expiry and a plot's level are worked out every time somebody asks and are never stored,
so a restart or a week offline needs no catching up.

## What a player earns, others gave

A rating counts only from somebody who stood in the build; visits count people, not doors; a level comes only from
what other players did. None of them can be farmed by the owner.

## Nothing is lost

A reset, a clear, a template and an expiry take a snapshot first; a contest winner is archived for good; a rent that
runs out takes away a right to build and never the build.

Related: [Plot worlds](../features/worlds.md), [Members and visitors](../features/members.md)
