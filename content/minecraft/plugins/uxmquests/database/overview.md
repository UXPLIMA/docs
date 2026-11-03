---
title: Overview
order: 901
description: What is stored, the tables, a network of servers, and moving off SQLite.
icon: database
---

The database holds each player's boards, their totals, the work on shared boards, pass standings and boosters. The
schema is created and upgraded on start; nothing is done by hand when a release changes it.

| Backend | For |
|---|---|
| [SQLite](sqlite.md) | One server. The default |
| [MySQL and MariaDB](mysql.md) | A network sharing quests |
| [PostgreSQL](postgresql.md) | The same, on PostgreSQL |

## Tables

Every table starts with `table-prefix` (`quests_`).

| Table | Holds |
|---|---|
| `quests_board` | One board per player per group: when it was handed out, rerolls used, whether the set reward was paid |
| `quests_assignment` | The quests on a board: phase, rule, and when it was claimed, for a catalogue cooldown |
| `quests_task` | Progress per task: done, needed, and the targets drawn for it |
| `quests_total` | Quests a player has finished per group, for milestones |
| `quests_contribution` | Who did what on a shared board, per group, quest and player |
| `quests_pass` | Pass experience per season and all time, and the tiers claimed on each track |
| `quests_booster` | Player and server boosters; one that has run out is deleted |
| `quests_schema_history` | The version the tables are at |

## A network

Point every server at the same database with the same `storage` block. Each server reads what the others wrote for the
players on it every `sync-seconds` (2): a board, a standing in a pass, the totals and the work on a shared board.

A proxy connects a player to the next server before the previous one has written them. `join-delay-ticks` (20 is one
second) makes that race rarer but does not close it; one shared database server does. A player counts nothing during
the wait.

## Keeping one server apart

`local-only = true` keeps one server on its own SQLite file, whatever `type` says: a creative world that has no business
in the shared database.

## Moving off SQLite

1. Create the database and a user.
2. Set `type`, `host`, `port`, `database`, `username`, `password`, and `copy-from-file = true`.
3. Restart once. The rows are copied into the empty tables, and the console says how many. Tables that already hold
   rows are left alone, so leaving it on copies nothing twice.

`/uxmquests doctor` says whether the backend answers.

Related: [config.conf](../config/config-conf.md#storage)
