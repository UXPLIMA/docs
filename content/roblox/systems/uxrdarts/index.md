---
title: uxrDarts
order: 25
description: Dartboards with four rule sets, an accuracy-based throw model, five AI tiers and an ELO ladder.
icon: target
---

Dartboards you place in the world. Walk up, pick a game, and throw. Four rule sets ship:
501 and 301, Around the Clock, Cricket and Shanghai.

The throw model is the interesting part. Where a dart lands is your aim plus a wobble, and
how big that wobble is depends on how accurately you have thrown in the past.

## Pages

- [Setup](setup.md)
- [Courts and boards](courts.md)
- [Rule sets](rule-sets.md)
- [Throwing](throwing.md)
- [The AI](ai.md)
- [Stats and leaderboards](stats.md)
- [Hooks](hooks.md)

## What it does

| | |
|---|---|
| Boards | Any number, found by tag |
| Rule sets | X01, Around the Clock, Cricket, Shanghai |
| Presets | Six configured matches, including a practice mode |
| Opponents | Another player, or an AI at five tiers |
| Scoring | A real dartboard: sectors, doubles, trebles and both bulls |
| Rating | ELO, plus wins, 180s and nine-darters |
| Leaderboards | In-world boards, global across servers |

## The board is a real board

Sector order, ring radii and scoring are the genuine article:

```lua
Config.Sectors = { 20, 1, 18, 4, 13, 6, 10, 15, 2, 17, 3, 19, 7, 16, 8, 11, 14, 9, 12, 5 }
```

That is the standard clockwise order starting at 20. The rings are proportional radii from
the centre out to the double wire, so a treble 20 is where a treble 20 is.

The consequence worth knowing: the numbers around a real board are arranged so that
missing 20 slightly puts you in 1 or 5. Any advice a player has ever heard about aiming for
the fat part of the board applies here too.
