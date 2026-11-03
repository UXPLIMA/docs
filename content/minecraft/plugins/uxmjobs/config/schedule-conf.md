---
title: schedule.conf
order: 608
description: Boosters written against a calendar.
icon: calendar-clock
---

| Key | Meaning |
|---|---|
| `enabled` | On or off |
| `multiplier` | An expression |
| `days` | `monday` to `sunday` |
| `start`, `end` | 24 hour times; an end before the start runs through midnight |
| `jobs`, `currencies` | Empty covers all |
| `broadcast` | A message key sent when it opens |

| Booster | Multiplier | Days | Hours | Ships |
|---|---|---|---|---|
| `weekend` | 1.5 | Saturday, Sunday | 00:00 to 23:59 | off |
| `evening` | 1.25 | Monday to Friday | 19:00 to 22:00 | off |

Read against the server machine's time zone.

Related: [Boosters](../features/boosters.md)
