---
title: Multiple servers
order: 37
description: One tournament across a whole network, with shared scores and rewards paid once.
icon: network
---

Servers that share one MySQL database can run one tournament together. A player can deposit on any
server, the scores are the same everywhere, and each reward is paid once, on whichever server the
winner is online.

## What you need

- One MySQL or MariaDB database for every server. SQLite cannot be shared, and read replicas are not
  supported.
- A Redis server, version 5.0 or newer. 6.2 or newer is recommended.
- The same uxmSupplyRush version on every server. A server on an older version refuses deposits and
  event changes until it is updated.
- Server clocks kept in sync, for example with NTP: run ends and scheduled starts follow each
  server's clock.

## Setting it up

In each server's `config.yml`:

```yaml
multi-server:
  enabled: true
  redis-host: "redis://user:password@10.0.0.5:6379"
  network: "default"
  server-name: "survival-1"
  stream-retention-seconds: 300
  stream-max-length: 100000
```

| Key | What to set |
|---|---|
| `redis-host` | The Redis address, the same on every server. Use `rediss://` for an encrypted connection |
| `network` | The same name on every server that shares the tournament |
| `server-name` | A different name on each server, 1 to 32 characters, shown in logs and messages |
| `stream-retention-seconds` | How long a server that lost Redis can catch up on what it missed (Redis 6.2 or newer) |
| `stream-max-length` | How much is kept for catching up on Redis older than 6.2 |

Redis must be reachable when a server starts, or uxmSupplyRush stays off on that server and says
why. If Redis goes down later, servers keep working, catch up within seconds once it is back, and
no deposit is lost or counted twice.

If you make a new server by copying another one's folder, delete
`plugins/uxmSupplyRush/data/server-id` in the copy before you start it.

## The management server

One server manages the tournament. The first server to start takes the role, says so in the
console and to admins, and keeps it across restarts.

| Only the management server | Every server |
|---|---|
| Creates, edits and deletes events | Takes deposits |
| Starts, pauses, stops and completes events | Warns its own players before the end |
| Ends a run when its time is up | Pays its own online winners |
| Fires the [weekly schedule](auto-start.md) | Shows the menus, leaderboards and history |

On any other server, those commands are refused with a message naming the management server.
`/sr management` shows which server it is, and `/sr management claim` moves the role to the server
you are on, after asking for confirmation. If the management server is down when a run's time is
up, deposits stop at the end time everywhere, and the run is finished as soon as it is back.

## Shared files

`events/<key>.yml`, `general-settings.yml` and `auto-start.yml` are the same on every server. Edit
them on the management server and run `/sr reload` there: the other servers update their own copy
within moments. Leave the `revision:` line at the top of these files alone.

An edit made on another server, or made to an outdated copy, is not applied. The edited file is
moved to `conflicts/` (or `events/conflicts/`) and the current version is written back, so nothing
you typed is lost.

`config.yml` and the language files stay separate on each server.

Related: [config.yml](../config/config-yml.md), [Database](../config/database.md)
