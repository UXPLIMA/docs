---
title: Spawner Types
order: 54
description: The spawners section of config.yml, the default type every mob falls back on, and item spawners.
icon: boxes
---

Every spawner type is an entry in the `spawners` section of `config.yml`. `default` is the one every
mob without an entry of its own is built from, so a fresh install already has a spawner for every
mob in the game.

```yaml
spawners:
  default:
    name: "<red><bold>{entity_name} <dark_gray>-- <gray>Spawner"
    disable-item-flags: true
    model-data: 0
    lore:
      - "<gray>When placed, this spawner will"
      - "<gray>gradually spawn {entity_name_lang}."
    spawnerInfo:
      mode: ENTITY
      material: {entity_name}
      range: 16
      delay: 8
      sell_button: true
```

## Keys

| Key | Meaning |
|---|---|
| `name`, `lore` | The spawner item's name and lore, in MiniMessage |
| `disable-item-flags` | Hides the item's extra tooltip lines |
| `model-data` | `ITEM` mode only: custom model data, or an item model such as `myserver:ruby`, for the item shown inside the spawner and the items it produces |
| `spawnerInfo.mode` | `ENTITY` spawns mobs, `ITEM` produces an item |
| `spawnerInfo.material` | A mob in `ENTITY` mode, such as `ZOMBIE`; an item in `ITEM` mode, such as `DIAMOND` |
| `spawnerInfo.range` | Blocks within which a player has to be for the spawner to run |
| `spawnerInfo.delay` | Seconds between cycles |
| `spawnerInfo.sell_button` | Whether the storage menu has a sell button |
| `spawnerInfo.details` | `ITEM` mode only: the `name` and `lore` of the items produced |

`{entity_name}` and `{entity_name_lang}` only work in `default`: the mob's name, and its translated
name from `translates` in the language file. Leave `material: {entity_name}` as it is there.

Write `range` and `delay` in every type. A type without `delay` uses the one from `default`, but a type
without `range` gets a range of 0 and practically never runs.

## Adding a type

1. Copy the whole `default` block under `spawners`.
2. Rename the key. It is the name `/uxmspawners give` takes, written exactly as here.
3. Change `name`, `lore` and `spawnerInfo` to what this type should be.
4. Run `/uxmspawners reload`.

```yaml
  diamond:
    name: "<aqua><bold>DIAMOND <dark_gray>-- <gray>Spawner"
    lore:
      - "<gray>When placed, this spawner will"
      - "<gray>gradually spawn diamonds."
    spawnerInfo:
      mode: ITEM
      material: DIAMOND
      range: 16
      delay: 8
      sell_button: true
      details:
        name: "<aqua>A cool diamond!"
        lore:
          - "<gray>This is a very cool diamond!"
```

A mob spawner with its own type, such as a slower `zombie`, works the same way with `mode: ENTITY` and
`material: ZOMBIE`.

## Changing a type later

| Change | Placed spawners |
|---|---|
| `delay`, `range`, `material`, `details`, `sell_button` | Follow the new value after a reload |
| `name`, `lore` | Keep the old ones on items already handed out, and those no longer level up with new ones |

Do not remove a type while spawners of it are placed: they read their settings from it on every
cycle.

## Item spawners

An `ITEM` spawner produces its item, level times per cycle, into its storage or onto the ground. It
has no auto-kill and no XP. The produced items sell by their material: a renamed diamond sells as a
diamond.

The comment above `details` in the shipped `config.yml` mentions `modules/settings.yml` and `force:`.
The setting is `force-default`, see [Storage & Selling](../features/storage.md). With virtual storage
off, the items drop around the spawner where players can see them.

## ItemsAdder

With ItemsAdder installed, `material` can name an ItemsAdder item or mob:

```yaml
    spawnerInfo:
      mode: ITEM
      material: "IA-myserver:ruby"
```

`IA-` followed by `namespace:id` produces that item in `ITEM` mode, or spawns that custom mob in
`ENTITY` mode.

Related: [Basic Concepts](../getting-started/concepts.md), [Prices](prices.md), [Loot Tables](loot-tables.md)
