---
title: Boosts
order: 36
description: Boost items that raise a spawner's output or speed for a while, how they stack and what they cost to define.
icon: zap
---

A boost is an item a player right-clicks onto a spawner. For a fixed time the spawner produces more
per cycle, or cycles faster. Boosts are defined in `modules/boosts.yml` and handed out with
`/uxmspawners giveboost <player> <boost>`, by hand or from a crate or store.

```yaml
boosts:
  enabled: true
  list:
    boost1:
      material: "DIAMOND"
      type: "SPAWN_AMOUNT"
      amount: 2
      model-data: 0
      duration: "1d"
      name: "<yellow><bold>BOOST <dark_gray>- <white>1 days"
      lore:
        - "<gray> Applying this boost will increase"
        - "<gray> the spawner's mob output by <green>+2"
```

| Key | Meaning |
|---|---|
| The key, `boost1` | The name `giveboost` takes and tab completion offers. `double-output` reads better than a number |
| `material` | The item. An unknown name gives paper |
| `type` | `SPAWN_AMOUNT` or `SPAWN_TIME` |
| `amount` | How strong it is, see below |
| `duration` | How long it lasts once applied: a number and one unit, `30s`, `45m`, `12h` or `1d` |
| `model-data` | Custom model data, or an item model such as `myserver:boost` |
| `name`, `lore` | The item's look, in MiniMessage |

`duration` takes one unit only. `1d12h` cannot be read and gives a boost that ends at once; write
`36h`.

## The two types

| Type | Effect | With `amount: 2` |
|---|---|---|
| `SPAWN_AMOUNT` | Adds `amount` to what one cycle produces, before the level multiplies it | Three times the output |
| `SPAWN_TIME` | Divides the time between cycles by `amount` | Twice as fast |

`SPAWN_AMOUNT` adds whole mobs: `2` means two extra mobs, or two extra loot rolls with auto-kill, so a
level 5 spawner with it produces as much as 15 separate spawners. A `SPAWN_TIME` of `1` does nothing,
and `0` stops the spawner until the boost ends.

A third type, `SELL_MULTIPLIER`, appears in the language file, but it has no effect in 4.2.0.

## Applying a boost

Hold the boost and right-click a spawner. One item is used and the boost starts.

| The player is told | Why |
|---|---|
| The boost was applied | It worked |
| A boost of that type is already active | One boost per type at a time. A `SPAWN_AMOUNT` and a `SPAWN_TIME` boost can run together |
| The spawner is not managed by uxmSpawners | It is a vanilla spawner |

Anyone can apply a boost to any spawner they can reach, not only their own. Active boosts survive
restarts, and the main menu shows each one with the time left.

`enabled: false` in this file does not stop boosts from working in 4.2.0. Remove the entries from
`list` to stop new boost items from being made.

<Callout type="tip" title="Boosts are the thing to sell">

A boost is temporary and used up, which makes it a far healthier crate or store item than a permanent
upgrade. The two shipped examples, two extra mobs per cycle for a day and four times the speed for
twelve hours, are a reasonable starting point.

</Callout>

Related: [Levels](levels.md), [Commands](../commands.md)
