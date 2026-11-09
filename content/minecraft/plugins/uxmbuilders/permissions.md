---
title: Permissions
order: 370
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

Numeric nodes take a value as their last part, and the highest a player holds wins. `.*` means no
limit.

| Node | Meaning | Default |
|---|---|---|
| `uxmbuilders.limit.<n>` | `n` builds at once, counting running, queued and paused | `uxmbuilders.limit.1` for everyone |
| `uxmbuilders.size.<n>` | Largest build, in blocks of volume | `uxmbuilders.size.10000` for everyone |
| `uxmbuilders.speed.<n>` | Builds `n` percent faster: `speed.50` is 1.5 times the builder's speed | none |
| `uxmbuilders.warehouse.pages.<n>` | Pages of the [warehouse](features/warehouse.md) window, at most 100 | one page |

`uxmbuilders.size.*` lifts the size limit. `uxmbuilders.speed.*` runs at the server ceiling,
`performance.blocksPerTick`; no speed goes past that ceiling.

Bukkit gives operators every node no plugin registered, and these are not registered, so an operator
holds `limit.*`, `size.*`, `speed.*` and `warehouse.pages.*`: no build limit, no size limit, the ceiling
speed and a hundred warehouse pages.

## Content

| Node | Grants |
|---|---|
| `uxmbuilders.package.<id>` | That package, whatever its `permission` and its category's `permission` say |
| `uxmbuilders.package.*` | Every package |
| `uxmbuilders.builder.<id>` | A builder at level 1, as if bought |
| `uxmbuilders.builder.<id>.level.<n>` | That builder at level `n` |
| `uxmbuilders.builder.*` | Every builder at level 1 |

These are registered at startup, and again on reload and whenever a package is created or deleted, with
operators **not** holding them, so staff testing the shop meet the same prices and levels players do.
Builder levels from permissions are read on every use: take the node away and the player is back at the
level they bought. A package's `permission`, a category's `permission` and a builder's
`unlock.permission` are nodes you name yourself.

## Bypasses

| Node | Skips | Default |
|---|---|---|
| `uxmbuilders.bypass.cost` | Paying for builds | operators |
| `uxmbuilders.bypass.cooldown` | The start cooldowns and the history teleport cooldown | operators |
| `uxmbuilders.bypass.limit` | `uxmbuilders.limit.<n>` | operators |
| `uxmbuilders.bypass.materials` | Spending warehouse materials | operators |
| `uxmbuilders.bypass.protection` | Land protection checks | operators |
| `uxmbuilders.bypass.restrictions` | Placement rules: worlds, heights, spawn distance | operators |

<Callout type="warning" title="Operators bypass protection by default">

`uxmbuilders.bypass.protection` lets the holder build on land they could not touch by hand. A staff
account that is also used to play keeps it; take it away with your permissions plugin if that is not
what you want. The node itself can be renamed with `protection.bypassPermission`.

</Callout>

## Market

The [player market](features/market/index.md)'s nodes. A category's `permission` in `market.yml` is a
node you name yourself.

| Node | Grants | Default |
|---|---|---|
| `uxmbuilders.market.use` | `market`, `browse`, `search`, `view`, `preview`, `designs` | everyone |
| `uxmbuilders.market.buy` | Buying designs | everyone |
| `uxmbuilders.market.sell` | `sell`, `mine`, `earnings`, `claim`: capturing and listing builds | everyone |
| `uxmbuilders.market.wand` | `wand`, `pos1`, `pos2`, `clear`, and marking corners with the wand | everyone |
| `uxmbuilders.market.report` | `report` | everyone |
| `uxmbuilders.market.fee.exempt` | Listing without the listing fee | operators |
| `uxmbuilders.market.notify` | Being told of reports, with a link to inspect the listing | operators |

## Market limits

Numeric, like the [limits](#limits) above.

| Node | Meaning | Without one |
|---|---|---|
| `uxmbuilders.market.listings.<n>` | `n` live listings at once; paused, sold-out and under-review listings count. `.*` is no limit | `listing.maxLiveDefault` |
| `uxmbuilders.market.size.<n>` | The largest selection one capture may hold, in blocks of volume; it never goes above `capture.maxVolume` | `capture.maxVolume` |
| `uxmbuilders.market.commission.<n>` | `n` percent commission on the player's sales, at most 100; of several held, the **lowest** wins | `economy.commissionPercent` |

`market.size.<n>` can only lower the volume `market.yml` allows: to sell bigger captures as a rank, give
your default group a lower value and the rank a higher one. A commission node is read when the player
lists a design, and the confirm menu shows the rate: the listing keeps it for every copy it sells, on
every server, online or not. A rank bought or lost later applies to the listings made after it.

Operators hold `market.listings.*` and `market.size.*`, as these nodes are not registered, but no
commission node: they pay the commission from `market.yml`.

## Market bypasses

| Node | Skips | Default |
|---|---|---|
| `uxmbuilders.market.bypass.protection` | The land check of a capture, and the wilderness rule | operators |
| `uxmbuilders.market.bypass.cooldown` | The waits between listings, purchases, reports and blueprints issued again | operators |
| `uxmbuilders.market.bypass.filter` | The word filter on titles, descriptions and sign text | operators |
| `uxmbuilders.market.bypass.limit` | `market.listings.<n>`, `market.size.<n>` and the copies-per-buyer limit; the size limits in `market.yml` still hold | operators |

## Admin

| Node | Grants | Default |
|---|---|---|
| `uxmbuilders.admin` | Every admin action, acting on any player's build, and update notices on join | operators |
| `uxmbuilders.admin.<action>` | One admin action: `reload`, `wand`, `save`, `package`, `give`, `build`, `cancel`, `setbuilder`, `protection`, `cache`, `perf`, `npc`, `debug`, `migrate` | through `uxmbuilders.admin` |
| `uxmbuilders.admin.market` | Every `/builders admin market` action: [market moderation](features/market/moderation.md) | through `uxmbuilders.admin` |

Give a moderator `uxmbuilders.admin.cancel` and `uxmbuilders.admin.perf` without handing them `give` or
`build`. Holding any one action's node, `uxmbuilders.admin.market` included, is enough for `admin` to
show in `/builders help`. The selection wand marks corners for holders of `uxmbuilders.admin.wand` or
`uxmbuilders.admin.save`.

A market moderator usually wants `uxmbuilders.admin.market` and `uxmbuilders.market.notify` together:
the first to decide, the second to hear of reports as they come in.

Related: [Commands](commands.md)
