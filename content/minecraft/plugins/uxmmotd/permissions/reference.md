---
title: Reference
order: 601
description: Every permission node of uxmMOTD, its default on each platform, and what it allows.
icon: list-checks
---

| Node | Paper default | Allows |
|---|---|---|
| `uxmmotd.admin.version` | op | `/uxmmotd` and `version` |
| `uxmmotd.admin.reload` | op | `/uxmmotd reload` |
| `uxmmotd.admin.maintenance` | op | `/uxmmotd maintenance` and its words |
| `uxmmotd.admin.doctor` | op | `/uxmmotd doctor` |
| `uxmmotd.maintenance.bypass` | not declared | Velocity only: joining while maintenance is on |

The four admin nodes are declared in `paper-plugin.yml` with the default `op`: each changes what every player who pings
the server sees, and maintenance closes the door. There is no `uxmmotd.admin.*` family; grant the nodes one by one or
use a permission plugin's wildcard.

Velocity has no defaults: the console may use every command and a player needs the node from a permission plugin. The
bypass node is read on Velocity only and is not declared anywhere; grant it from a permission plugin.

Related: [Maintenance](../features/maintenance.md)
