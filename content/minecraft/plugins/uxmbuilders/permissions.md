---
title: Permissions
order: 270
description: Every node, what it grants, and what to give your default group.
icon: key-round
---

Players get everything they need by default. Ranks are sold through the numeric nodes, builders and
packages; staff need `uxmbuilders.admin`.

## Commands

| Node | Grants | Default |
|---|---|---|
| `uxmbuilders.use` | `/builders`, `menu`, `help`, `confirm` | everyone |
| `uxmbuilders.command.shop` | The shop | everyone |
| `uxmbuilders.command.preview` | `preview`, `rotate`, `mirror`, `offset` | everyone |
| `uxmbuilders.command.start` | `start` | everyone |
| `uxmbuilders.command.list` | `list` and the Active Builds menu | everyone |
| `uxmbuilders.command.status` | `status` | everyone |
| `uxmbuilders.command.pause` | `pause` and `resume` | everyone |
| `uxmbuilders.command.cancel` | `cancel` | everyone |
| `uxmbuilders.command.crew` | `crew` | everyone |
| `uxmbuilders.command.warehouse` | `warehouse` | everyone |
| `uxmbuilders.command.trust` | `trust` | everyone |
| `uxmbuilders.command.boost` | `boost` | everyone |
| `uxmbuilders.command.history` | `history` | everyone |
| `uxmbuilders.history.others` | Another player's history | operators |
| `uxmbuilders.history.teleport` | Teleport from the history | operators |

## Limits

The highest node a player holds wins.

| Node | Meaning | Default |
|---|---|---|
| `uxmbuilders.limit.<n>` | `n` builds at once, counting running, queued and paused | `uxmbuilders.limit.1` for everyone |
| `uxmbuilders.size.<n>` | Largest build, in blocks of volume | `uxmbuilders.size.10000` for everyone |
| `uxmbuilders.speed.<n>` | `n` percent faster builds | none |

`uxmbuilders.size.*` lifts the size limit. `uxmbuilders.speed.*` runs at the server ceiling,
`performance.blocksPerTick`.

## Content

| Node | Grants |
|---|---|
| `uxmbuilders.package.<id>` | A package, when its `permission` is set to this node |
| `uxmbuilders.package.*` | Every package |
| `uxmbuilders.builder.<id>` | A builder, as if bought |
| `uxmbuilders.builder.<id>.level.<n>` | That builder at level `n` |
| `uxmbuilders.builder.*` | Every builder |

These are registered at startup with operators **not** holding them, so staff testing the shop meet the
same prices and levels players do. A builder's `unlock.permission` and a category's `permission` are
nodes you name yourself.

## Bypasses

| Node | Skips | Default |
|---|---|---|
| `uxmbuilders.bypass.cost` | Paying for builds | operators |
| `uxmbuilders.bypass.cooldown` | The start cooldown and the history teleport cooldown | operators |
| `uxmbuilders.bypass.limit` | `uxmbuilders.limit.<n>` | operators |
| `uxmbuilders.bypass.materials` | Spending warehouse materials | operators |
| `uxmbuilders.bypass.protection` | Land protection checks | operators |
| `uxmbuilders.bypass.restrictions` | Placement rules: worlds, heights, spawn distance | operators |

<Callout type="warning" title="Operators bypass protection by default">

`uxmbuilders.bypass.protection` lets the holder build on land they could not touch by hand. A staff
account that is also used to play keeps it; take it away with your permissions plugin if that is not
what you want.

</Callout>

## Admin

| Node | Grants | Default |
|---|---|---|
| `uxmbuilders.admin` | Every admin action, and acting on any player's build | operators |
| `uxmbuilders.admin.<action>` | One admin action: `reload`, `save`, `package`, `give`, `build`, `cancel`, `setbuilder`, `protection`, `cache`, `perf`, `npc`, `debug`, `migrate` | through `uxmbuilders.admin` |

Give a moderator `uxmbuilders.admin.cancel` and `uxmbuilders.admin.perf` without handing them `give` or
`build`.

Related: [Commands](commands.md)
