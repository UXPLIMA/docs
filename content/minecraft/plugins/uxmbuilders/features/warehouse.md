---
title: Warehouse
order: 205
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
| `/builders warehouse give <owner> [all]` | The same, into the warehouse of a player who [trusts](trusted-players.md) you |

Items with a name, lore or enchantments are refused and handed back: the store keeps a material and a
count, and storing them would strip what makes them special.

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
| `materials.pauseTimeout` | `600` | Seconds of waiting before the job is cancelled with a partial refund |
| `materials.notifyInterval` | `60` | Seconds between reminders |

The timeout only counts while the owner is online and their warehouse has loaded: time spent offline,
when they could not restock, is not held against them.

## The materials list

The job menu's **Material List** shows what is still to be placed and how much of it is missing, with
substitutions counted. The shop's materials button shows the whole package before buying.

## Notes

- `uxmbuilders.bypass.materials` builds without spending materials. It is read when the job starts
  and again on every resume.
- Balances live in memory and are written on the save interval, when the player leaves, and at
  shutdown. A failed write is retried.
- Items taken out of an open warehouse menu are held aside, so a crash with the menu open loses nothing.

Related: [Builders](builders.md), [Trusted players](trusted-players.md)
