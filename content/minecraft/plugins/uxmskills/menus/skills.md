---
title: Skills windows
order: 402
description: The skills window, a skill, levels, sources, abilities, stats, rate and top.
icon: zap
---

## Skills

Skills in 10 to 16 and 19 to 25 (`skills:skills`). Left click opens one (`skills:open`), right click its board
(`skills:top`). Stats at 38, rate at 40, the overall board at 42.

## A skill

| Slot | Button |
|---|---|
| 13 | Summary: level, experience, progress |
| 20 | Levels (`skills:levels`) |
| 22 | Sources (`skills:sources`) |
| 24 | Abilities (`skills:abilities`) |
| 31 | Top (`skills:top`) |
| 40 | Back |

Conditions: `skills:finished`, `skills:unfinished`.

## Levels

Slots 0 to 44 (`skills:levels`): what each level pays; `skills:level-reached` marks the ones reached.

## Sources

Slots 0 to 44 (`skills:sources`): the price list; the sorter at 47 (`skills:sort`) moves it to the next order.

## Abilities

Slots 10 to 34 (`skills:abilities`): each with its level and value. Conditions: `skills:ability-open`,
`skills:ability-locked` (not yet reached), `skills:ability-suppressed` (a conflicting ability holds it back). Abilities
are bought by command, not by click. Back at 49 (`skills:open-skill`).

## Stats

Slots 10 to 25 (`skills:stats`).

## Rate

Slots 10 to 25 (`skills:terms`): every anti-farm term pulling the rate down. `skills:full-rate` shows the tile at 13
when nothing is.

## Top

Standings in 10 to 43 (`skills:standings`); `skills:nobody` shows the empty tile when nobody has earned yet.

Related: [Player commands](../commands/skills.md)
