---
title: Networks
order: 1140
description: Regions that belong to one server or are shared by several, and how an edit reaches every server.
icon: network
---

Guard runs the same on one server and on a network of many. On a network, every server keeps its own regions
apart, and regions you want everywhere (one lobby spawn for five lobby servers) live in a shared set.

## What you need

All servers point uxmEssentials at **the same database** (MySQL, MariaDB or PostgreSQL in `config.conf`; a SQLite
file belongs to one server). Each server needs its own `network.server-name` in `config.conf`, since that name is
how guard tells servers apart.

## Scopes

Every region has a scope: which servers load it.

| Scope | Loaded by |
|---|---|
| `server:<name>` | Only the server with that `network.server-name`. The default for a new region |
| `set:<name>` | Every server that lists the set in `network.shared-sets` |

```hocon
# modules/guard/config.conf on each lobby server
network {
  shared-sets = ["lobbies"]
  new-region-scope = "server"
}
```

```text
/guard scope spawn set:lobbies
/guard scope spawn server
```

With `new-region-scope = "set:lobbies"`, regions you make on that server go into the set from the start.
`/guard info` shows a region's scope as "applies on". A server ignores regions of scopes it does not load, so
a `server:` region of another server, or a set it does not list, costs it nothing.

## How an edit travels

1. The edit is written to the database, history included, in one transaction.
2. The server publishes "this region changed" on the uxmEssentials network bus (`network.enabled` with the
   Velocity or Redis transport, see [Cross-Server](../cross-server/index.md)).
3. Every server that loads that scope reads the region again and recompiles that world.

When the bus is off, or a message is lost (a Velocity bus needs a player online to carry it), every server also
checks the database every `network.poll-seconds` (10) for regions changed since it last looked, so edits arrive
within that time at worst.

## Good to know

- A region's shape, rules and roles are the same on every server that loads it; its world must exist there, and
  a server without that world simply has nothing to protect.
- `/guard bypass`, inspect mode and the borders you see are per server, as they belong to your session.
- History is shared: `/guard history` on any server shows a set region's changes from every server.
- [Backups](tools.md#backups) are written by each server for the regions it loads.
