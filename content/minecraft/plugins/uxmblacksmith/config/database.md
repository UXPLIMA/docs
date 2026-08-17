---
title: The database
order: 306
description: SQLite, the eight tables, and what to back up.
icon: database
---

One SQLite file, `database.db`, in the plugin folder. There is no other backend and nothing to
install.

## Connection

| | |
|---|---|
| Driver | SQLite via HikariCP |
| Pool size | 4 |
| Journal mode | WAL |
| Synchronous | NORMAL |

WAL means a second file, `database.db-wal`, sits next to the database and holds recent writes.
**Copy both**, or copy neither — a `database.db` taken alone while the server runs can be missing the
last few minutes.

Writes go through an operation queue off the main thread, and per-player work is serialised under a
lock, so two clicks cannot interleave into a corrupt state.

## The tables

| Table | Holds |
|---|---|
| `pending_trades` | Running crafts: slot, trade id, timing, refund payload |
| `active_boosts` | Running boosts with their absolute expiry |
| `player_profiles` | Level, XP, perk points, craft counts |
| `player_masteries` | One row per player per mastery |
| `player_perks` | One row per player per perk level |
| `progression_events` | Every XP grant, keyed for duplicate protection |
| `trade_events` | The full audit trail |
| `trade_daily_stats` | Per-day per-trade rollups |
| `pending_refunds` | Refunds owed but not yet delivered |

Indexed by player, trade id and timestamp — the queries the analytics commands run stay fast as the
event log grows.

## Migrations

Columns are added in place on start when they are missing. Upgrading the plugin does not need a
manual step and does not lose data. There is no downgrade path: a database written by a newer build
may carry columns an older jar does not know about.

## What a restart does

Pending trades are rows, not memory. A restart resumes every craft with the right amount of time
elapsed, because progress is stored as real timestamps rather than ticks. A player who logs out
mid-craft comes back to a finished one.

The same applies to boosts: a 60-minute `TIME` boost expires 60 minutes after activation whether the
server was up or not.

## Backups

Back up the whole `plugins/uxmBlacksmith/` folder:

- `database.db` **and** `database.db-wal` — every player's progression and every pending craft
- `binary_files/` — the saved items your trades reference
- `categories/`, `config.yml`, `gui.yml`, `language.yml` — the configuration

`binary_files/` matters more than it looks: a trade referencing a saved item that has gone missing
is skipped at load, so losing that folder silently removes trades from the menu.

<Callout type="warning" title="Do not edit the database by hand while the server is up">

WAL plus a live connection pool means an external writer can produce a state the plugin does not
expect. Stop the server, take a copy, then work on the copy.

</Callout>

## Growth

Nothing prunes `trade_events` or `progression_events`. They are append-only and grow with activity.
That is deliberate — they are the audit trail — but plan an archive if you run for years. Deleting
old rows from `trade_events` does not affect gameplay; `trade_daily_stats` already holds the
aggregates the analytics commands read.
