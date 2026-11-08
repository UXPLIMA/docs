---
title: Warehouse
order: 305
description: A per-player material store that construction draws from, block by block.
icon: warehouse
---

With `materials.enabled: true`, every placed block takes one matching item from its owner's warehouse.
The warehouse is a store kept in the database, not a chest in the world: players fill it from
`/builders warehouse` and the build draws from it wherever the site is.

The warehouse is off by default; builds then cost money only.

## Filling it

| Way | What moves |
|---|---|
| Drag items into `/builders warehouse` | Any plain item, page by page |
| `/builders warehouse deposit` | The stack in hand |
| `/builders warehouse deposit all` | Every plain item in the inventory that places a block |
| `/builders warehouse <owner>` | An empty window into the warehouse of a player who [trusts](trusted-players.md) you; what you leave in it is theirs when you close it |
| `/builders warehouse give <owner> [all]` | The same as `deposit`, into the warehouse of a player who trusts you |

Items with a name, lore or enchantments are refused and handed back: the store keeps a material and a
count, and storing them would strip what makes them special. Only the owner can take items out.

## Pages

The window shows `storageSlots` of `menu/warehouse.yml` per page, and `uxmbuilders.warehouse.pages.<n>`
decides how many pages a player sees: one without the node, a hundred at most. A page is a view, not a
cap: stock past the last page stays in the warehouse, is still spent by builds, and is never lost.

## What a block costs

| Block | Costs |
|---|---|
| Most blocks | One of its item |
| Double slabs, candles, sea pickles, turtle eggs, petals, snow layers | One item per piece |
| Wall torches, redstone wire, crops, cocoa | The item that places them: a torch, redstone, seeds, beans |
| Upper half of a door or tall plant, head of a bed | Nothing; the lower half paid for the item |
| Dirt path, farmland | Dirt |
| Blocks no survival player can obtain (budding amethyst, reinforced deepslate, infested stone) | Nothing |

A block whose exact state already stands is skipped and costs nothing. A builder's `efficiency` is the
chance a block costs nothing at all.

`materials.substitutions` lets one item stand in for another:

```yaml
materials:
  substitutions:
    minecraft:oak_planks: [ minecraft:spruce_planks, minecraft:birch_planks ]
```

## When it runs out

The job pauses with the reason **out of materials**, and the owner is told which item and how many they
hold. It checks again every second and resumes on its own once the item is there.

| Key | Default | Meaning |
|---|---|---|
| `materials.pauseTimeout` | `600` | Seconds of waiting before the job is cancelled, refunded as an owner cancel (`economy.refund.onCancel`); `0` waits for ever |
| `materials.notifyInterval` | `60` | Seconds between reminders |

The timeout only counts while the owner is online and their warehouse has loaded: time spent offline,
when they could not restock, is not held against them.

## The materials list

The job menu's **Material List** shows what is still to be placed and how much of it is missing, with
substitutions counted. Shift-clicking a package in the shop, or the materials button of the purchase
confirmation, shows the whole package before buying.

## Notes

- `uxmbuilders.bypass.materials` builds without spending materials. It is read when the job starts
  and again on every resume, including the automatic one after a restock.
- [Site preparation](site-preparation.md) charges one item of the foundation block for each foundation
  block it lays.
- Balances live in memory and are written every 30 seconds, when the player leaves, and at shutdown. A
  failed write is kept and tried again, and does not stop the other warehouses from being saved.
- The page on screen is held while the window is open: a build cannot spend what the player is looking
  at, yet it is still saved with the warehouse, so a crash or a stop with the window open loses nothing.
  Closing the window or turning the page settles it.

Related: [Builders](builders.md), [Trusted players](trusted-players.md)
