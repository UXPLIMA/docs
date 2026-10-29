---
title: Upgrading to 4.0.0
order: 13
description: What to do before and after moving a 3.x.x server to uxmFarmer 4.0.0.
icon: circle-arrow-up
---

4.0.0 moves every sale, withdrawal and upgrade into the database, under a lock on the farmer. The
configuration carries over: new keys are added with their defaults on the first start, and the
database schema is updated by itself.

## Before you start

1. Check the server: Paper 1.21.11 or newer, on Java 21.
2. Back up `plugins/uxmFarmer/` and, on MySQL, the database. The first start merges duplicate
   product and statistics rows and adds new columns; the backup is your way back to 3.x.x.
3. On a network, stop every server and upgrade them all together. 4.0.0 renamed its Redis channels,
   so a 3.x.x server and a 4.0.0 server do not hear each other and overwrite each other's farmers.

## What changes on the first start

| Area | 3.x.x | 4.0.0 |
|---|---|---|
| Farmer logs | `log-cleaner` kept 50 and then dropped 25 | `farmer-logs` keeps the last 100, none older than 30 days. Older logs are deleted on the first start |
| Log files | Every farmer log was also written to the log file | Only with `write-farmer-logs-to-file: true`. Files rotate daily and are kept 14 days |
| Saving | A full save every five minutes | Collected products are written every `flush-interval-seconds`; money and items move in the database at once |
| Products removed from `collected-materials.yml` | Their stock was deleted on reload | Their stock is kept, and the console names them |

The old `log-cleaner` block is no longer read. Set the new limits in `farmer-logs` before the first
start if 100 logs and 30 days are too few for you.

## What players and staff notice

| Change | Where it is documented |
|---|---|
| Bred, egg and dispenser mobs, and built golems, are never auto-killed. Babies give no loot or XP. Loot ignores Looting | [Auto Kill](../features/auto-kill.md) |
| Farmer eggs no longer stack: each one is its own item | [Commands](../commands.md#giving-items) |
| `only-owners-can-upgrade` also covers Upgrade All | [Storage and Levels](../features/storage-and-levels.md#upgrade-all) |
| Only the owner can transfer a farmer | [Members and Ranks](../features/members-and-ranks.md#transferring-and-deleting) |
| Switching modules, collection and products takes `ACCESS_MANAGE_MENU`; conversions take `SELL` | [Members and Ranks](../features/members-and-ranks.md#rank-permissions) |
| `/uxmfarmer teleport` takes `uxmfarmer.command.adminteleport` | [Permissions](../permissions.md#admin-command-permissions) |
| Tax and upgrade discounts stop at 100% | [Permissions](../permissions.md#tiered-permissions) |
| A farmer that cost nothing refunds no farmer price | [general-settings.yml](../config/general-settings.md#prices-and-limits) |

Related: [Installation](installation.md), [Database and multi-server](../config/database.md),
[Logs](../config/logs.md)
