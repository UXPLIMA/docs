---
title: Basic Concepts
order: 14
icon: lightbulb
---

## A spawner is an item with an owner

A uxmSpawners spawner is a normal block carrying NBT that names its type. Placing it registers it
to the player who placed it; that ownership is what the menus, the friends list and the breaking
rules all check.

## Storage instead of drops

By default a spawner does not litter the ground. Mobs are killed by the auto-kill module, their
drops go into the spawner's own virtual storage, and the player opens a menu to sell or withdraw
them. XP is stored the same way.

`modules/settings.yml` decides whether new spawners start with virtual storage and XP storage on,
and whether players may change it:

```yaml
virtual-storage:
  default: true
  force-default: false
  permission: uxmspawners.settings.togglestorage
```

With `force-default: true` the choice is yours, not theirs.

## Two modes: ENTITY and ITEM

A spawner type spawns either a mob or an item:

```yaml
spawnerInfo:
  mode: ENTITY    # or ITEM
  material: ZOMBIE
  range: 16
  delay: 8
```

`ENTITY` uses the material as an entity type; `ITEM` uses it as an item and produces that item
directly, which is how the shipped `diamond` spawner works.

## Modern auto-kill

With `use-modern-autokill.enabled: true` on 1.16+, nothing is spawned at all. The plugin resolves
what the mob *would* have dropped and adds it to storage. No entity is created, killed or ticked,
which is the difference between a spawner farm the server notices and one it does not.

<Callout type="tip" title="Loot tables apply here">

`modules/autokill.yml → loottable-only-autokill` is on by default: your custom loot tables apply
to auto-killed mobs, while a mob a player kills by hand drops what vanilla says. Turn it off to
make loot tables apply to both.

</Callout>

## Levels and boosts

A spawner has a level, capped by `modules/other.yml → spawner-level-limit.level-limit` (256 by
default). On top of that, boost items apply a temporary multiplier to either the amount spawned or
the delay between spawns. Boosts are consumable items you hand out or sell.
