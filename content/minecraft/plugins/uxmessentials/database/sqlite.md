---
title: SQLite (Default)
order: 1560
description: 'The default backend: no setup, one file, and when it stops being enough.'
---

uxmEssentials ships with an embedded SQLite database and uses it out of the box: nothing to install,
nothing to run alongside the server, no credentials to manage.

**It just works.** A fresh install is already storing everything in SQLite. Unless you run a network of
backend servers or a very busy economy, you can leave the `storage` block exactly as
it ships and never think about databases again.

## Why SQLite?

| Pros                              | Cons                                            |
|-----------------------------------|-------------------------------------------------|
| Zero setup: works immediately  | Not suited to a multi-server network          |
| No external software to run     | A single file, awkward to back up while live  |
| Fast for small to medium servers   | No remote access                              |
| Survives world rollbacks        | Writes are serialized (one at a time)         |

**Recommended for:** most single servers, especially under ~100 concurrent players.

## Configuration

Everything lives in the `storage` block of `plugins/uxmEssentials/config.conf`. SQLite is
the shipped default, so a new install already looks like this:

```hocon
storage {
  backend = "sqlite"            # sqlite | mysql | postgres
  file = "uxmessentials.db"     # SQLite only: the database file name
  # host / port / database / username / password apply to network backends only
  read-pool-size = 8            # network backends only; SQLite is fixed single-writer
  connection-timeout-ms = 5000
}
```

For SQLite only two keys matter: `backend = "sqlite"` and `file`, the name of the database
file. The remaining keys are read only when you switch to a network backend.

## Where the File Lives

The database is created inside the plugin's data folder:

```
plugins/uxmEssentials/data/uxmessentials.db
```

That single file holds all of your persisted data: economy wallets, homes, warps, vaults,
moderation records, and more. It runs in **WAL (write-ahead logging)** mode, so you will see
two sidecar files appear next to it:

```
plugins/uxmEssentials/data/uxmessentials.db-wal
plugins/uxmEssentials/data/uxmessentials.db-shm
```

These are normal. WAL keeps reads fast while a write is in progress and is cleaned up by
SQLite automatically.

## Single-Writer Behaviour

SQLite allows many readers but only **one writer at a time**. uxmEssentials handles this for
you: the write connection is pinned so writes are serialized into a queue, while a small pool
of read connections serves lookups concurrently. You never have to tune this.

**One writer, on purpose.** Because SQLite serializes writes, an extremely write-heavy server (large networks, a shop
economy processing constant transactions) can eventually feel it. This is the point at
which you move to [MySQL / MariaDB](mysql.md) or [PostgreSQL](postgresql.md), which use
real row-level locking. It is a topology decision, not a bug: SQLite is not a "degraded"
mode.

What is stored in the database survives world rollbacks. Economy balances in particular are
**always** DB-backed and never written to entity/player data: a rolled-back world can never
give someone their money back. Only throwaway state such as cooldown and kit-claim stamps
lives outside the database.

## When SQLite Is Enough

| Your situation                                | Use SQLite? |
|-----------------------------------------------|-------------|
| One server, any typical player count          | Yes       |
| A quiet or medium economy                     | Yes       |
| You do not want to run a database server      | Yes       |
| Several backends sharing one player base      | No, see below |
| Constant, heavy concurrent writes             | Consider a network backend |

A network of servers **cannot** share a SQLite file: each server would hold its own copy.
Cross-server sync requires a shared MySQL/MariaDB or PostgreSQL database. See
[Cross-Server](../cross-server/overview.md).

## Backing Up

The cleanest backup is taken while the server is stopped:

1. **Stop the server** so no writes are in flight.
2. Copy `data/uxmessentials.db` (and, if present, the `-wal` and `-shm` sidecars) somewhere safe.
3. Start the server again.

<Callout type="warning" title="Back up before switching backends">

Moving from SQLite to a network backend starts a **fresh** schema on the new database;
it does not copy your existing data across. Back up `uxmessentials.db` first, and plan how
you will carry data over (or accept a clean slate) before you change `backend`.

</Callout>

Related: [config.conf (Globals)](../config/global-config.md), [MySQL / MariaDB Setup](mysql.md), [PostgreSQL Setup](postgresql.md), [Cross-Server](../cross-server/overview.md)
