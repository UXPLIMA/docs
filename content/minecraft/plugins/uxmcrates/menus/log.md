---
title: Log
order: 510
description: "The opening log window: every win that matches, with the chance it had."
icon: scroll-text
---

`/uxmcrates log <player|crate|reward> <value>` from a player (`uxmcrates.admin.log`).

## Slots

| Slot | Tile | Click |
|---|---|---|
| 9 to 44 | One win each, a paper, newest first | Nothing |
| 48 | Arrow | The previous page |
| 50 | Arrow | The next page |
| Every other slot | Grey pane | Nothing |

## A win

| Line | From |
|---|---|
| Title | The player's name |
| Crumb | `A win` |
| Description | `menu.log.description` |
| Crate | The crate's name |
| Reward | The reward's name |
| Chance at the time | The chance the reward had when it was won |
| When | The date in `date-format` |

Related: [The log and housekeeping](../features/log-and-housekeeping.md)
