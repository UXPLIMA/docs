---
title: Concepts
order: 203
description: Groups, boards, assignments, phases, claiming, sharing and passes.
icon: lightbulb
---

| Word | Means |
|---|---|
| Group | One file under `groups/`; the file name is its id. A set of quests with one mode, reset and claim rule |
| Board | What a player holds of one group: the quests drawn or started for them |
| Assignment | One quest on a board, with the numbers drawn for it when it was handed out |
| Task | One thing a quest asks for, of one [type](../features/task-types.md), with an amount |
| Phase | Where a quest stands: not started, in progress, complete and unclaimed, claimed |
| Cycle | A group that draws a fresh board on every reset |
| Catalogue | A group that offers every quest and lets the player start the ones they want |
| Claim | `automatic` pays a finished quest at once; `manual` waits for `/quests claim` or a click |
| Sharing | `solo` counts each player's own work; `contribution` makes one board for the whole server |
| Pass | A ladder of tiers under `passes/`, fed by finished quests and by raw play |
| Track | The free rewards of a tier, and the premium ones behind a permission |

A span such as `"64-128"`, or a draw such as `one-of`, is chosen once when the quest is handed out and stored with it,
so a reload never changes the number under a player halfway through. A player whose data has not loaded counts
nothing; nothing waits on the database on a server thread.

Related: [Groups and boards](../features/groups-and-boards.md)
