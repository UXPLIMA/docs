---
title: Lifecycle
order: 410
description: Resets, inactive islands, backups and restores, and what a player carries.
icon: recycle
---

## Resets

The owner resets an island with `/is reset`, after a second confirmation, and starts again from a preset. The island
and everything it wrote is cleared. What a reset, a leave, a kick or a death does to a player (empty the inventory or the
ender chest, take the experience, send them to spawn, or on death keep the inventory) is set rule by rule in
[lifecycle.conf](../config/lifecycle-conf.md), per game mode and per ruleset. A player who is offline when it happens
pays it at their next join, on any server. How fast an island is
cleared so the server never stalls is in [performance.conf](../config/performance-conf.md).

## Inactive islands

Once a day the plugin looks for islands whose people stopped playing, as [inactivity.conf](../config/inactivity-conf.md)
ships it:

| When | What happens |
|---|---|
| The owner has not played for 30 days | The island passes to the next member in the order Co-Owner, Moderator, Member, and the former owner becomes a Co-Owner |
| No member has played for 60 days | The island is archived |

`/is admin inactivity scan` looks now.

## Backups and restores

`/is admin backup <island>` keeps a copy of one island; `/is admin backup database` copies the whole database. Island
backups are kept on this server's disk, in an S3 bucket, or both; see `island-backups` in
[config.conf](../config/config-conf.md). `/is restore <backup> [mode]` puts an island back; see
[Staff](../commands/admin.md) for the modes.

## What a player carries

A player's inventory and state are saved as they play, and kept per kind of world: what they carry on a Brix plot or a
Parkour course is not what they carry elsewhere. See [player-state.conf](../config/player-state-conf.md).
