---
title: Spawner Types
order: 54
description: The spawners folder, the default type every mob falls back on, types of your own and item spawners.
icon: boxes
---

Every spawner type is a file in `spawners/`, and the file name is its name: `diamond.yml` is the
`diamond` type. `default.yml` is the one every mob without a file of its own is built from, so a
fresh install already has a spawner for every mob in the game.

```yaml
name: "<#7AD7C9><bold><entity_name_lang> Spawner"
disable-item-flags: true
model-data: 0

lore:
  - "<#5F6778>Spawns <#DCE1EA><entity_name_lang> <#5F6778>while a player is near."
spawner-info:
  mode: ENTITY
  material: "{entity_name}"
  range: 16
  delay: 8
  sell-button: true
```

## Keys

| Key | Meaning |
|---|---|
| `name`, `lore` | The spawner item's name and lore, in MiniMessage |
| `disable-item-flags` | Hides the item's extra tooltip lines |
| `model-data` | `ITEM` mode only: a custom model data number for the items it produces, or 0 |
| `spawner-info.mode` | `ENTITY` spawns mobs, `ITEM` produces an item |
| `spawner-info.material` | A mob in `ENTITY` mode, such as `ZOMBIE`; an item in `ITEM` mode, such as `DIAMOND` |
| `spawner-info.range` | Blocks within which a player has to be for the spawner to run. 16 when left out |
| `spawner-info.delay` | Seconds between cycles. 8 when left out |
| `spawner-info.sell-button` | Whether the storage menu has a **Sell All** button |
| `spawner-info.details` | `ITEM` mode only: the `name` and `lore` of the items produced |

In `name` and `lore`, `<entity_name>` is the mob's name, `ZOMBIE`, and `<entity_name_lang>` its
translated name from `translates` in the language file. Leave `material: "{entity_name}"` as it is in
`default.yml`: it stands for the mob the spawner is given for.

## A type of your own

1. Copy `default.yml` in `spawners/`.
2. Name the copy after a mob, such as `zombie.yml`, to change every spawner of that mob, the ones
   natural spawners break into included. Or give it a new name, such as `iron.yml`, for a spawner
   you hand out with `/usp give <player> iron`.
3. Change `name`, `lore` and `spawner-info` to what this type should be. A copy named after a mob can
   keep `material: "{entity_name}"`.
4. Run `/usp reload`.

`spawners/diamond.yml`, shipped as an example:

```yaml
name: "<#8FE3F0><bold>Diamond Spawner"
disable-item-flags: true
lore:
  - "<#5F6778>Makes <#8FE3F0>diamonds <#5F6778>while a player is near."
spawner-info:
  mode: ITEM
  material: DIAMOND
  range: 16
  delay: 8
  sell-button: true
  details:
    name: "<#7AD7C9>A cool diamond!"
    lore:
      - "<#9AA3B5>This is a very cool diamond!"
```

File names are matched without regard to case, so `Iron.yml` and `iron.yml` are one type. Do not
keep both.

## Changing a type later

| Change | Placed spawners |
|---|---|
| `delay`, `range`, `material`, `details`, `sell-button` | Follow the new value after a reload |
| `name`, `lore` | Keep the old ones on items already handed out, and those no longer level up with new ones |
| The file is deleted or renamed | Its spawners run on `default.yml` and look like it. They can still be broken, and work as before once the file is back |

`default.yml` and `diamond.yml` are written again when they are missing. To retire the diamond
spawner, keep the file and stop handing it out.

## Item spawners

An `ITEM` spawner produces its item, level times per cycle, into its storage or onto the ground. It
has no Auto Kill and no XP, and its menu hides those buttons. With Virtual Storage off, the items drop
around the spawner where players can see them. See [Storage & Selling](../features/storage.md).

## ItemsAdder

With ItemsAdder installed, `material` can name an ItemsAdder item or mob:

```yaml
spawner-info:
  mode: ITEM
  material: "IA-myserver:ruby"
```

`IA-` followed by `namespace:id` produces that item in `ITEM` mode, or spawns that custom mob in
`ENTITY` mode. ItemsAdder mobs have no loot table to roll, so they are spawned and killed even with
modern Auto Kill.

Related: [Basic Concepts](../getting-started/concepts.md), [Prices](prices.md), [Loot Tables](loot-tables.md)
