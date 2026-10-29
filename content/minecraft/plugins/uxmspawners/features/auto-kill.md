---
title: Auto-Kill
order: 34
description: Turning a spawner into a farm nobody has to stand at, the modern and classic ways it works, and the mobs that do spawn.
icon: swords
---

With auto-kill on, a spawner kills what it spawns and puts the drops into its storage, so the farm
runs while the owner is away from it, as long as someone is within its range. It only applies to
`ENTITY` spawners.

## Turning it on

Auto-kill is switched per spawner, with the **Auto-Kill** button in its menu. `modules/autokill.yml`:

```yaml
auto-kill:
  enabled: true
  enable-for-split-entities: false
  force: false
  loottable-only-autokill: true
  permission: uxmspawners.autokill
```

| Setting | Meaning |
|---|---|
| `permission` | What the owner, or a friend, needs to press the button |
| `force` | Every new spawner starts with auto-kill on, and the button can only turn it on |
| `enabled` | Read together with `force`: new spawners start with auto-kill on only when both are `true` |
| `enable-for-split-entities` | The small slimes and magma cubes a killed one splits into are killed too |
| `loottable-only-autokill` | Not read in 4.2.0. Loot tables apply to every mob from a spawner, see [Loot Tables](../config/loot-tables.md) |

Without `force`, a new spawner starts with auto-kill off, and its owner turns it on. `force` does
not change spawners already placed: one that has it off keeps it off until someone presses the
button. `enabled: false` does not hide the button; to hide it, set `gui: false` on the `auto-kill`
item in the language file.

## Modern and classic

```yaml
use-modern-autokill:
  enabled: true
```

in `config.yml`.

| | Modern | Classic |
|---|---|---|
| A mob appears | No | Yes, and dies on the same tick |
| Drops | The mob's loot table is rolled directly | The mob's drops when it dies |
| XP | From your loot table, otherwise 1 to 5 | What the mob gives |
| Cost to the server | Nothing but the roll | A mob spawned and killed every cycle |
| Split slimes | Not relevant | Killed only with `enable-for-split-entities` |

Leave modern on. Both pick a random online player as the killer, so loot that needs a player kill
drops.

With a [loot table](../config/loot-tables.md) for the mob, both use it. Without one, modern rolls the
game's own loot table for that mob and classic uses what the mob dropped.

## Mobs that do spawn

Without auto-kill, or with classic auto-kill, mobs exist in the world. Two settings in `config.yml`
keep them cheap and harmless:

```yaml
nerf-spawner-mobs:
  enabled: false
  nerf-every-mob: false
  mobs:
    - ZOMBIE

no-equipment:
  enabled: false
```

| Setting | Meaning |
|---|---|
| `nerf-spawner-mobs` | The mobs lose their AI: they do not move, look around, target or attack. A nerfed creeper does not explode |
| `nerf-every-mob: false` | Only the mobs in `mobs` are nerfed |
| `nerf-every-mob: true` | Every mob is nerfed except those in `mobs` |
| `no-equipment` | Spawned mobs carry no armour or weapons, so they cannot drop them either |

A spawner mob killed by a player drops into the spawner's storage, multiplied by the level, like an
auto-killed one. Food drops come out cooked when the mob was on fire.

Related: [Basic Concepts](../getting-started/concepts.md), [Loot Tables](../config/loot-tables.md)
