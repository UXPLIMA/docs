---
title: Boosts
order: 136
description: Boost items that raise a spawner's output, speed or sell price for a while, and how they are defined.
icon: zap
---

A boost is an item a player right-clicks onto a spawner. For a fixed time the spawner produces more
per cycle, cycles faster, or sells for more. Boosts are defined in `boosts.yml` and handed out with
`/usp giveboost <player> <boost>`, by hand or from a crate or store.

```yaml
enabled: true

list:
  boost1:
    material: "DIAMOND"
    type: "SPAWN_AMOUNT"
    amount: 2
    model-data: 0
    duration: "1d"
    name: "<#A99BF0><bold>Spawn Amount Boost</bold> <#5F6778>· <#DCE1EA>1 day"
    lore:
      - "<#5F6778>Each spawn cycle makes <#A99BF0>+2 <#5F6778>more."
```

| Key | Meaning |
|---|---|
| The key, `boost1` | The name `giveboost` takes and tab completion offers. `double-output` reads better than a number |
| `material` | The item. An unknown name gives paper |
| `type` | `SPAWN_AMOUNT`, `SPAWN_TIME` or `SELL_MULTIPLIER` |
| `amount` | How strong it is, see below. Above 0 |
| `duration` | How long it lasts once applied: a number and one unit, `30s`, `45m`, `12h` or `1d` |
| `model-data` | A custom model data number for resource packs, or 0 for none |
| `name`, `lore` | The item's look, in MiniMessage |

`duration` takes one unit only: write `36h`, not `1d12h`. A boost whose type, amount or duration
cannot be read is refused when used, with a warning in the console, and the item is kept.

A boost item stores only its key. What it does is read from `boosts.yml` when it is used, so editing
an entry changes the items already handed out. Renaming or removing the key makes them useless.

## The three types

| Type | Effect | With `amount: 2` |
|---|---|---|
| `SPAWN_AMOUNT` | Adds `amount` to what one cycle produces, before the level multiplies it | Three times the output |
| `SPAWN_TIME` | Divides the time between cycles by `amount` | Twice as fast |
| `SELL_MULTIPLIER` | Multiplies the money from selling this spawner's storage, on top of the seller's own multiplier | Twice the money |

A fractional `SPAWN_AMOUNT` counts as a chance: `1.5` adds one or two, half the time each.

## Applying a boost

Hold the boost and right-click a spawner you own or are a friend of. One item is used and the boost
starts.

| The player is told | Why |
|---|---|
| The boost was applied | It worked |
| A boost of that type is already active | One boost per type at a time. Boosts of different types run together |
| The spawner is not managed by uxmSpawners | It is a natural spawner |
| They have no access | It is someone else's spawner |
| The item was not made by the server | It was made or edited by hand. See [Item Signing](item-signing.md) |

Active boosts survive restarts, and the main menu shows each one with the time left.
`enabled: false` stops boost items from working and stops running boosts from counting until it is
turned on again.

<Callout type="tip" title="Boosts are the thing to sell">

A boost is temporary and used up, which makes it a far healthier crate or store item than a permanent
upgrade. The two shipped examples, two extra mobs per cycle for a day and four times the speed for
twelve hours, are a reasonable starting point.

</Callout>

Related: [Levels](levels.md), [Storage & Selling](storage.md), [Commands](../commands.md)
