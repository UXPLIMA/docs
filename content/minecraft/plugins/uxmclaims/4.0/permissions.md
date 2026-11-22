---
title: Permissions
order: 150
description: The kinds of permission nodes, the kernel's own nodes, and where each module lists its nodes.
icon: key-round
---

Every node starts with `uxmclaims.` and is registered with the server when its module starts, with a
default and a description, so LuckPerms can list it. A node of a module that is off does not exist.
This page explains the kinds of nodes. Each module's page lists its own.

## Kinds of nodes

| Pattern | What it controls | Usual default |
|---|---|---|
| `uxmclaims.command.<module>.<command>` | Whether a player may run a command at all | everyone |
| `uxmclaims.ability.<thing>.<action>` | Whether a player may do an action where their land role allows it, such as `uxmclaims.ability.warp.create` | everyone |
| `uxmclaims.admin.<module>` | Staff commands and actions of one module | op |
| `uxmclaims.bypass.<what>` | Skipping one rule, such as a cooldown, a combat tag or a land's entry check | op |
| `uxmclaims.limit.<name>.<n>` | A number, such as lands or chunks per player | read through [Limits](modules/limits.md) |
| `uxmclaims.delay.<name>.<n>` | A wait in seconds, such as the teleport warm-up | read through [Limits](modules/limits.md) |
| `uxmclaims.cost.<name>.<price>` | A price, when the cost in `costs.yml` names that node | read through [Economy](modules/economy.md) |

Command and ability nodes answer "may this player use the feature". Inside a land, the player's
role decides the rest: a member whose role lacks `warps:manage` cannot create a warp there, whatever
nodes they hold.

## Kernel nodes

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.admin` | op | `/uxm` (modules, reload, setup, presets, lang, migrate) and every admin action; also bypasses protection when `protection.admin-bypass` is `IMPLICIT` |
| `uxmclaims.admin.db` | op | `/uxm db status`, `repair`, `baseline` and `remap-world` |
| `uxmclaims.admin.bypass` | op | The admin bypass switch when `protection.admin-bypass` is `TOGGLE` |
| `uxmclaims.bypass.enter` | op | Walking into lands that refuse you |
| `uxmclaims.bypass.teleport` | op | Teleporting, ender pearls and chorus fruit into lands that refuse you |
| `uxmclaims.migration.bypass` | op | Joining while the 3.x data is copied in the background |

The commands that use these are on [Migration commands](upgrading/migration-commands.md) and
[Configuration](configuration/index.md).

## Land roles are not permission nodes

What members, visitors and allies may do inside a land is set per land, in its roles, with keys such
as `lands:block_break` or `warps:use`. Owners change them in the land menu. A module adds its own
keys when it starts: Warps adds `warps:use` and `warps:manage`, Land Vault adds its own, and so on.
The keys and the roles every land starts with are on [Lands](modules/lands.md).

## Limits, costs and delays

A limit node carries its value in the name: `uxmclaims.limit.chunk.25` lets a player have 25
chunks. How several nodes combine (stack, highest, lowest) is set per limit in
`modules/limits/limits.yml`; `<node>.*` removes the limit. Prices live in
`modules/economy/costs.yml`, and a cost answers a `uxmclaims.cost.*` node only when its entry names
one. See [Limits](modules/limits.md) and [Economy](modules/economy.md).

With [LuckPerms](modules/luckperms.md) on, limits are also read for offline players, and LuckPerms
contexts such as `uxmclaims:in-land` let you grant a node only inside lands.

## Ability nodes and LuckPerms

Ability nodes default to everyone. Under LuckPerms, an ability node no group sets follows
`undefined-ability` in `modules/luckperms/config.yml`. It ships as `allow`, also on a server moved
from 3.x. Set it to `deny` to get the 3.x behaviour back, and then grant `uxmclaims.ability.*` or the
single nodes to your default group, or nobody can claim. The setting is read by lands, areas, towns
and nations; see [LuckPerms](modules/luckperms.md) for the details.

## Where each module lists its nodes

| Module | Node prefixes |
|---|---|
| [Lands](modules/lands.md) | `command.lands.*`, `ability.*`, `admin.lands`, `limit.claim`, `limit.chunk`, `limit.member`, `limit.role` |
| [Towns](modules/towns.md) | `command.towns.*`, `ability.town.*`, `admin.towns` |
| [Nations](modules/nations.md) | `command.nations.*`, `ability.nation.*`, `admin.nations` |
| [Areas](modules/areas.md) | `command.areas.*`, `ability.area.*`, `admin.areas` |
| [Warps](modules/warps.md) | `ability.warp.*`, `admin.warps`, `limit.warp`, `delay.warprename`, `delay.warprelocate` |
| [Combat](modules/combat.md) | `command.combat.admin`, `bypass.combat` |
| [War](modules/war.md) | `command.war.use`, `ability.war.declare`, `admin.war`, `bypass.war.commands` |
| [Jails](modules/jails.md) | `command.jails`, `admin.jails`, `bypass.jail`, `bypass.outlaw` |
| Any other module | `command.<module>` and `admin.<module>`, listed on its page under [Modules](modules/index.md) |

Related: [Limits](modules/limits.md), [Economy](modules/economy.md), [LuckPerms](modules/luckperms.md), [Lands](modules/lands.md)
