---
title: Spawner Types
order: 54
description: Defining a spawner type in ENTITY or ITEM mode.
icon: boxes
---

Every spawner in the game is one entry in the `spawners` section. `default` is the fallback: any
mob without its own entry is built from it.

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

## Adding a type

1. Copy the whole `default` block.
2. Rename it: the key is the id `/uxmspawners give` takes.
3. Change `material` and whatever else differs.

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

## Keys

| Key | Meaning |
|---|---|
| `name` | Item name, MiniMessage. `{entity_name}` is the entity |
| `lore` | Item lore. `{entity_name_lang}` is the translated entity name |
| `disable-item-flags` | Hides vanilla item flags on the spawner item |
| `model-data` | Custom model data |
| `spawnerInfo.mode` | `ENTITY` spawns creatures, `ITEM` produces items |
| `spawnerInfo.material` | An entity type in `ENTITY` mode, an item in `ITEM` mode |
| `spawnerInfo.range` | Blocks a player must be within for the spawner to run |
| `spawnerInfo.delay` | Seconds between spawn attempts |
| `spawnerInfo.sell_button` | Whether the menu offers selling |
| `spawnerInfo.details` | `ITEM` mode only (the name and lore of the produced item) |

<Callout type="warning" title="Leave material alone in default">

`material: {entity_name}` in the `default` block is what makes one definition cover every mob. A
literal material there turns every unconfigured spawner into that one thing.

</Callout>

<Callout type="tip" title="Seeing ITEM spawners actually drop">

An `ITEM` spawner's output goes to virtual storage like everything else. To see items falling
around the spawner, set `virtual-storage.default: false` and `force: true` in
`modules/settings.yml`, which disables virtual storage for all spawners.

</Callout>
