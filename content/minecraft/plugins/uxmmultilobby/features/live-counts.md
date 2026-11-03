---
title: Live counts
order: 301
description: The three sources of a lobby's count, how often they refresh, and when an answer stops being trusted.
icon: activity
---

`status.source` in [config.conf](../config/config-conf.md#status) chooses where the counts come from.

## proxy

Asks the proxy over the `BungeeCord` plugin messaging channel. Costs no socket and needs no firewall rule, but the proxy
reports no maximum, so `maximum` from `lobbies.conf` is used, and it reports zero for a lobby that has crashed, so a dead
lobby looks like an empty one.

## ping

Opens a socket to each lobby's `host` and `port` and reads its server list ping: the count and the maximum it reports.
The only source of the three with nothing else to set up that tells a dead lobby from an empty one. The lobbies' ports
must be reachable from every lobby.

## redis

Listens on `status.redis.channel` for frames the lobbies publish themselves, written as
`<lobby-id>;<players>;<maximum>`. A maximum of `0` means the configured one. It needs `lettuce-core` on the server;
without it the plugin logs a warning and falls back to `proxy`.

## Timing

| Key | Default | Meaning |
|---|---|---|
| `interval-ms` | `5000` | How often the counts refresh. The query runs on the async scheduler and the window reads a cache, so this never delays a click |
| `timeout-ms` | `1500` | How long a ping waits for one lobby. A timeout is not an error: the lobby is down |
| `stale-after-ms` | `15000` | How long an answer is trusted. A lobby that has not answered within this is shown offline. `0` trusts an answer for ever, which suits `redis` only |

A lobby is full when its count reaches its maximum. Before its first answer a lobby is unknown, and shows "Checking".

Related: [Concepts](../getting-started/concepts.md#states), [Placeholders](../placeholders/reference.md)
