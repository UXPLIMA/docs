---
title: End rules
order: 35
description: The rules that decide what kind of fight happens in the event world, and the block and item blacklists.
icon: scale
---

While an event runs, the plugin changes how the event world behaves. Each rule is independent, and
together they decide what kind of event you are running.

```yaml
end-rules:
  disablePvP: true
  disableDamage: true
  disableCrystalDamage: true
  keepInventory: true
  announceAdvancements: false
  showDeathMessages: false
  generateStructures: false
  disableDragonEgg: true
  disablePickups: false
  disableBlockBreak: false
  disableItemDrop: false
  disableBrokenBlockDrops: false
  disableBlockPlace: false
  disableBreathPickup: false
  disablePotions: false
  disableElytra: false
  disableDragonDestroy: false
  disableCrystalDestroy: false
  disableEndermanGrief: false
```

## What the defaults mean

The shipped settings make a **co-operative fight with no risk**: nobody can hurt anybody, nobody takes
damage, and nobody loses items. Everyone is there to damage the dragon and climb the leaderboard.

That removes every reason for the event to turn into an argument. A fight where players can kill each
other for the last hit is a different event, and one to turn on deliberately.

## The rules

| Rule | When `true` |
|---|---|
| `disablePvP` | Players cannot hurt each other, arrows and other projectiles included |
| `disableDamage` | Players in the event world take no damage at all, from the dragon or anything else |
| `disableCrystalDamage` | End crystal explosions do not hurt players |
| `keepInventory` | Players keep their items when they die in the event |
| `announceAdvancements` | Advancements are announced in chat. `false` hides them |
| `showDeathMessages` | Death messages are shown. `false` hides them |
| `generateStructures` | End cities generate in a generated event world. `false` keeps them out |
| `disableDragonEgg` | No dragon egg appears when the dragon dies |
| `disablePickups` | Players cannot pick items up |
| `disableItemDrop` | Players cannot drop items |
| `disableBlockBreak` | Players cannot break blocks |
| `disableBrokenBlockDrops` | Blocks players break drop nothing |
| `disableBlockPlace` | Players cannot place blocks or empty buckets |
| `disableBreathPickup` | Players cannot use glass bottles, so dragon's breath cannot be collected |
| `disablePotions` | Players cannot drink or throw potions |
| `disableElytra` | Players cannot glide with an elytra |
| `disableDragonDestroy` | The dragon does not break the blocks it flies through |
| `disableCrystalDestroy` | End crystal explosions do not break blocks |
| `disableEndermanGrief` | Endermen do not pick up or place blocks |

`announceAdvancements`, `showDeathMessages` and `generateStructures` read the other way round from
the `disable` rules: they are on when `true`. These three, and `keepInventory`, are applied when the
plugin generates or copies the event world; a `type: VANILLA` world keeps its own game rules. See
[Worlds](worlds.md#game-rules-in-the-event-world).

The rules about items, blocks, potions and gliding apply to players in the event. Staff in the event
world without joining are not held to them.

## Rules worth thinking about

**`disableDragonEgg: true`.** Three events a day would put three eggs a day into the economy, and the
egg stops being a trophy within a week. Give it as a first-place reward in `rewards.yml` instead, as
the default rewards do.

**`disableDamage: true`.** With this on, the dragon's `damage-modifier` does nothing. Turn it off
before tuning damage, and keep `keepInventory: true` if dying should not cost players their gear.

**Dying leaves the event.** A player who dies is out of the event and comes back through the
entrance, paying again if `price-required-for-every-join` is on. With damage on, a death costs more
than the items.

**`disableBlockPlace`.** Left off, players build up to reach the dragon, which is part of the vanilla
fight. If you only want to stop certain blocks, use `block-blacklist` instead.

**`disableElytra: false`** gives anyone with an elytra the flight that is otherwise a permission. Set
it to `true` unless your arena allows for that.

**`disableDragonDestroy: false`** lets the dragon carve up the arena. Harmless in a world that is
rebuilt before every event; not harmless in a `type: VANILLA` world, which is kept.

## Block and item blacklists

```yaml
block-blacklist:
  - "*_BED"
  - "TNT"

item-blacklist:
  - "MACE"
```

| List | Effect on players in the event |
|---|---|
| `block-blacklist` | These blocks cannot be placed, even with `disableBlockPlace: false`. `LAVA` and `WATER` also stop the buckets |
| `item-blacklist` | These items cannot be used, and hitting with one in the main hand does nothing |

Write material names in capitals. A `*` at the start or the end matches the rest of the name: `*_BED`
covers every bed colour, `NETHERITE_*` every netherite item.

Beds and TNT are blocked by default because both explode in the end. The mace is blocked because its
damage grows with the distance fallen, and in a dragon fight that distance has no limit.

Blacklisting what breaks the fight is better than turning building off entirely: the list stays
short, and the parts of the fight that work stay as they are.
