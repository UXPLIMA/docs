---
title: Quests and board
order: 502
description: The group list and a group's board, with their tokens, phases and actions.
icon: scroll
---

## Quests

`/quests`. Groups in slots 10 to 16 (`quests:groups`), each a written book; a click opens its [board](#board)
(`quests:open`). Paging at 18 and 26. The star at 22 (`quests:passes`) opens the [passes](passes.md) window and is drawn
only while a pass runs (`quests:has-passes`).

## Board

Four rows. The quests in 10 to 16 and 19 to 25 (`quests:board`), the back feather at 31, paging at 27 and 35.

| Click | Action | Does |
|---|---|---|
| Left | `quests:claim` | Takes the quest forward: starts one that is waiting, pays a finished one |
| Right | `quests:reroll` | Swaps it for another from the same pool |

`quests:start` (start a catalogue quest and nothing else) and `quests:back` are there for your own buttons.

| Token | Means |
|---|---|
| `%group%`, `%name%`, `%description%` | The group or quest and its words |
| `%quest%` | The quest's id |
| `%done%`, `%needed%`, `%total%` | Progress, the amount, quests on the board |
| `%objectives%` | Each task's line |
| `%state%` | The state in the reader's language |
| `%phase%` | `not-started`, `in-progress`, `complete-unclaimed` or `claimed` |
| `%phase-material%` | The material `menu.phase-display` gives the phase |
| `%appendix%` | `menu.lore-appendix` |

The condition `quests:phase phase=<phase>` gates an item on the phase, for a different click line or a lock icon.

## Phase materials

`menu.phase-display` in `config.conf`:

| Phase | Material |
|---|---|
| `base` | `PAPER` |
| `not-started` | `BOOK` |
| `in-progress` | `WRITABLE_BOOK` |
| `complete-unclaimed` | `ENCHANTED_BOOK` |
| `claimed` | `GRAY_DYE` |

Related: [Groups and boards](../features/groups-and-boards.md)
