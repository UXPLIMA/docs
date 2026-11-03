---
title: network.conf
order: 505
description: The transport, the server name and the Redis settings.
icon: network
---

| Key | Default |
|---|---|
| `transport` | `none` (or `sql`, `redis`) |
| `server-name` | `""` |
| `poll-seconds` | 2 |
| `keep-minutes` | 30 |
| `redis-uri` | `""` |
| `channel` | `uxmplots` |
| `server-stale-minutes` | 3 |
| `handoff-seconds` | 60 |

What travels and how is in [Network and Discord](../features/network-and-discord.md#the-network).

Related: [Database](../database/overview.md)
