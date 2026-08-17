---
title: End rules
order: 35
description: The twenty rules that decide what kind of fight happens inside.
icon: scale
---

Inside the event world the plugin overrides vanilla behaviour. Each rule is independent, and together
they decide what kind of event you are running.

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

The shipped configuration is a **co-operative fight with no risk**: nobody can hurt anybody, nobody
takes damage, and nobody loses items. Everyone is there to damage the dragon and get on the
leaderboard.

That is the right default because it removes every reason for the event to become an argument. The
alternative (a fight where players can kill each other for the last hit) is a different event, and
one you should turn on deliberately.

## The rules

| Rule | When `true` |
|---|---|
| `disablePvP` | Players cannot damage each other |
| `disableDamage` | Players take no damage at all, including from the dragon |
| `disableCrystalDamage` | End crystal explosions do not hurt players |
| `keepInventory` | Items are kept on death |
| `announceAdvancements` | *(false hides them)* Advancements are announced |
| `showDeathMessages` | *(false hides them)* Death messages are shown |
| `generateStructures` | *(false disables)* End cities generate |
| `disableDragonEgg` | No dragon egg spawns when the dragon dies |
| `disablePickups` | Players cannot pick items up |
| `disableBlockBreak` | Players cannot break blocks |
| `disableItemDrop` | Players cannot drop items |
| `disableBrokenBlockDrops` | Broken blocks drop nothing |
| `disableBlockPlace` | Players cannot place blocks |
| `disableBreathPickup` | Dragon's breath cannot be bottled |
| `disablePotions` | Potions cannot be used |
| `disableElytra` | Elytra cannot be used |
| `disableDragonDestroy` | The dragon does not break blocks it flies through |
| `disableCrystalDestroy` | Crystals cannot be destroyed |
| `disableEndermanGrief` | Endermen do not move blocks |

Note that `announceAdvancements`, `showDeathMessages` and `generateStructures` read the opposite way
from the `disable...` rules: they are on when `true`.

## Rules worth thinking about

**`disableDragonEgg: true`.** One egg per event, and the event runs three times a day. Left enabled,
the egg stops being a trophy within a week. Give it as a first-place reward in `rewards.yml`
instead, which the default rewards do.

**`disableDamage: true`.** With this on, the dragon's `damage-modifier` settings do nothing. Turn it
off before tuning damage, and remember that `keepInventory: true` is what stops death from being a
real loss.

**`disableBlockPlace`.** Left off, players will build up to reach the dragon, which is fine, and
part of the vanilla fight. If you do not want it, `block-blacklist` is the softer tool: it blocks
specific materials without stopping building entirely.

**`disableElytra: false`** means anyone with elytra has flight in an event where flight is otherwise
a permission. Set it to `true` unless your arena accounts for that.

**`disableDragonDestroy: false`** lets the dragon carve up your arena. Harmless in a world that gets
reset; not harmless if you built something you want back.

## Block and item blacklists

```yaml
block-blacklist:
  - "*_BED"
  - "TNT"

item-blacklist:
  - "MACE"
```

Blocks that cannot be placed and items that cannot be used inside the event, regardless of the
`disableBlockPlace` rule. `*` matches: `*_BED` covers every bed colour.

Beds and TNT are blacklisted by default because both explode in the end. The mace is there because
its damage scales with fall distance, which in a dragon fight is unbounded.

Add anything that breaks the fight rather than turning off building altogether: it is a shorter list
and it leaves the parts of the fight that work alone.
