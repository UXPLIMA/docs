---
title: Upgrading to 1.1.0
order: 14
description: What happens to a 1.0.0 server's settings, events and running tournament on the first start of 1.1.0.
icon: circle-arrow-up
---

Replacing the jar is all an upgrade takes. On its first start, 1.1.0 moves your 1.0.0 setup into the
new layout by itself, and a tournament that was running carries on where it stopped.

## Before you start

1. Back up `plugins/uxmSupplyRush/`, and on MySQL, the database. With SQLite the plugin also copies
   the database file to `backups/` before changing it.
2. Stop the server, replace the jar, and start it again.

## What moves on the first start

| 1.0.0 | 1.1.0 |
|---|---|
| The `auto-start` section of `config.yml` | `auto-start.yml` |
| The `log-cleaner` section of `config.yml` | `general-settings.yml` |
| `status`, `start-time`, `end-time` and `paused-time` in each event file | Kept by the plugin itself; the lines are removed from the files |
| `autostart_data.yml` (this month's automatic starts) | Kept by the plugin itself |

A running or paused event keeps its run, its scores and its remaining time. The console lists each
step as it happens. A setting that is already in its new file is left as it is, so starting the
server twice changes nothing.

## New in 1.1.0

One tournament can now run across several servers that share a MySQL database. Nothing changes for
a single server; see [Multiple servers](../features/multi-server.md) to set it up.

Related: [Installation](installation.md), [Database](../config/database.md)
