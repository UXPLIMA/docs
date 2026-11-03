---
title: A network
order: 303
description: Sharing the choice through one database, instant changes through Redis, and keeping one server apart.
icon: network
---

## The database carries the choice

Every backend reads the player's row while they log in, which is before the first line they read is drawn. Point every
server at the same MySQL, MariaDB or PostgreSQL database with the same `table-prefix`, and a player who picked a
language on one server reads it on the next.

## Redis tells the others at once

Without Redis, a server that already holds the player reads the new choice at their next login. With
`network.transport = "redis"`, a change is published at once and the server the player is on switches without
waiting. A change for a player who is on another server is skipped there and read from the row when they arrive. A lost
message costs nothing, because the row is the truth.

| Key under `network` | Default | Meaning |
|---|---|---|
| `transport` | `"none"` | `none` for one server, `redis` for a network |
| `server-name` | `""` | What this server calls itself on the channel; empty makes a new id on every start, which is enough |
| `redis.host`, `port` | `"127.0.0.1"`, `6379` | The Redis server |
| `redis.user`, `password` | `""` | Leave the user empty for a Redis with `requirepass` |
| `redis.database` | `0` | For a Redis shared with something else |
| `redis.ssl` | `false` | For a Redis reached over TLS |
| `redis.channel` | `"uxmlang"` | The channel; two plugins sharing one would read each other's news |

Write the same `network.redis` block on every server.

## Keeping one server apart

`storage.local-only = true` on one server keeps its rows in its own SQLite file, whatever `type` says: a creative or
build server with no business in the shared database. Keep the storage block otherwise the same on every server.

Related: [Database](../database/overview.md), [config.conf](../config/config-conf.md#network)
