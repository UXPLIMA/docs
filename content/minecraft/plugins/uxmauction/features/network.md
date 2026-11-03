---
title: A network
order: 312
description: One house on a shared database, with the sql or redis transport.
icon: network
---

For several servers, point every `storage` block at one MySQL, MariaDB or PostgreSQL database with the same
`table-prefix`, and choose a transport so a window open on one server stops showing a listing somebody bought on
another.

| `network.transport` | How |
|---|---|
| `none` | One server |
| `sql` | Polls the shared database every `poll-every` (1s); needs nothing else |
| `redis` | Tells the other servers the moment something changes; needs a Redis server. The driver is in the jar |

A message carries no item and no money: it says which row is old news, and the server that reads it goes to the
database. A lost message costs a window one refresh.

| Key under `network` | Default | Meaning |
|---|---|---|
| `server-name` | `""` | What this server calls itself; empty makes a new id on every start, which is enough |
| `poll-every` | `"1s"` | For `sql` |
| `keep-for` | `"5m"` | How long a message stays in the table; longer than `poll-every` |
| `redis.host`, `port`, `user`, `password`, `database`, `ssl` | `127.0.0.1`, `6379` | For `redis` |
| `redis.channel` | `"uxmauction"` | Two houses on one Redis need two channels |

The lock is shared: one command closes every server. `storage.local-only = true` keeps one server's house apart.

Related: [Database](../database/overview.md), [config.conf](../config/config-conf.md#network)
