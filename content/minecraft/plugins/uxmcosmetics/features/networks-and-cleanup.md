---
title: Networks and cleanup
order: 310
description: Sharing wardrobes across servers, keeping one server apart, and sweeping leftover entities.
icon: network
---

## A network

Point every server at the same database with the same `table-prefix`, and a player's owned and worn pieces follow
them. `storage.sync-seconds` (2) is how often a server reads what the others changed for the players on it. Keep the
family files and `boxes.conf` the same on every server: a piece one server does not define is not drawn there.

`storage.local-only = true` keeps one server on its own SQLite file, such as a build server.

## Cleanup

Every follower is marked non-persistent, so it is never saved. A crash can still leave one written to disk:

| Way | When |
|---|---|
| `cleanup.sweep-on-start = true` | The loaded chunks are swept at start and every follower whose owner is not online is removed |
| `/uxmcosmetics cleanup` | The same sweep, now, with the count in `admin.cleaned` |

Related: [Database](../database/overview.md), [Followers](followers.md)
