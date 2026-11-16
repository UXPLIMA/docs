---
title: Spawner Types
order: 154
description: The spawners folder, the four sections of a spawner file, the default type every mob falls back on, custom blocks and item spawners.
icon: boxes
---

Every spawner type is a file in `spawners/`, and the file name is its name: `diamond.yml` is the
`diamond` type. `default.yml` is the one every mob without a file of its own is built from, so a
fresh install already has a spawner for every mob in the game.

A file has four sections: the item players hold, the block it places, what the spawner does, and
how its hologram looks.

```yaml
enabled: true

item-settings:
  material: SPAWNER
  model: 0
  name: "<#7AD7C9><bold><entity_name_lang> Spawner"
  lore:
    - "<#5F6778>Spawns <#DCE1EA><entity_name_lang> <#5F6778>while a player is near."
  glowing: false
  enchantments: {}
  hide-flags: true

block-settings:
  material: SPAWNER
  show-inside: true

spawner:
  mode: ENTITY
  produces:
    spawns: "<entity_name>"
    model: 0
    name: ""
    lore: []
  spawn-conditions:
    players:
      range: 16
      min: 1
      max: 0
  delay: 8
  sell-button: true

hologram: {}
```

## enabled

`false` makes the plugin act as if the file were not there: spawners of this type already placed
make nothing until it is turned on again, and their menu says why. They can still be broken.
`default.yml` is always used, whatever it says.

## item-settings: the spawner item

What `/usp give` gives and a broken spawner drops.

| Key | Meaning |
|---|---|
| `material` | `SPAWNER`, any other block item that places one block of its own, or an ItemsAdder (`IA-namespace:id`) or Nexo (`NEXO-id`) custom block's item. Not a door, bed, tall plant, chest or head, nor a slab, candle, snow layer or other block that joins one placed before |
| `model` | A custom model data number, or an item model such as `myserver:spawner`; 0 for none |
| `name`, `lore` | In MiniMessage. `<entity_name_lang>` is the mob's name in the server's language, `<entity_name>` its name in the game, `ZOMBIE`. Without a `name`, the file uses `default.yml`'s |
| `glowing` | The enchantment glint |
| `enchantments` | Enchantments and their levels, such as `{unbreaking: 3}`; a datapack's as `"namespace:key": 1` |
| `hide-flags` | Hides the item's tooltip details: enchantments, attributes and the rest |

## block-settings: the placed block

| Key | Meaning |
|---|---|
| `material` | `SPAWNER`, or a plain full block that does not fall, has no use of its own and does not explode, such as `DIAMOND_BLOCK`; or an ItemsAdder or Nexo custom block (not a Nexo string block) |
| `show-inside` | `SPAWNER` only: the mob or item it makes spins inside the cage |

A material that does not fit is reported in the console on load, and `SPAWNER` is used. A spawner of
another block behaves like a spawner: it is protected the same way, and right-clicking it opens its
menu. A wet sponge spawner cannot be placed in the Nether, where it would dry at once.

## spawner: what it does

| Key | Meaning |
|---|---|
| `mode` | `ENTITY` spawns mobs, `ITEM` makes an item |
| `produces.spawns` | A mob in `ENTITY` mode, such as `ZOMBIE`, or an ItemsAdder mob, `IA-namespace:id`; an item in `ITEM` mode, such as `DIAMOND` |
| `produces.model`, `produces.name`, `produces.lore` | `ITEM` mode only: how the item it makes looks. Empty is the item's own look |
| `spawn-conditions` | When it makes something. See [Spawn Conditions](../features/spawn-conditions.md) |
| `delay` | Seconds between cycles. At least 1 |
| `sell-button` | Whether its storage can be sold from the menu |

Leave `spawns: "<entity_name>"` as it is in `default.yml`: there it stands for the mob each spawner
was given for. A `spawns` that is no mob or no item is reported in the console with its file, and
those spawners make nothing.

## hologram: its own look

Any setting of `hologram.yml` written here is used for this type's spawners instead of
`hologram.yml`'s: `content`, `height`, `background-color`, `scale`, `translation`, `billboard`,
`text-alignment`, `shadow`, `see-through-blocks`, `text-opacity` and `brightness`. `enabled` and
`view-distance` are set in `hologram.yml` only. `{}` takes everything from `hologram.yml`.

```yaml
hologram:
  height: 2
  content:
    - "<spawner> <#5F6778>· <#A99BF0>Lv. <level>"
```

See [Holograms](../features/holograms.md).

## A type of your own

1. Copy `default.yml` in `spawners/`.
2. Name the copy after a mob, such as `zombie.yml`, to change the zombie spawners given and dropped
   from now on, natural spawners broken into one included. Or give it a new name, such as
   `iron.yml`, for a spawner you hand out with `/usp give <player> iron`.
3. Change the sections to what this type should be. A copy named after a mob can keep
   `spawns: "<entity_name>"`.
4. Run `/usp reload`.

File names are matched without regard to case, so `Iron.yml` and `iron.yml` are one type. Do not
keep both.

## Changing a type later

| Change | Placed spawners |
|---|---|
| Anything in `block-settings` and `spawner` | Follow the new values after a reload, the block included: a placed spawner turns into the new block |
| `item-settings` | New items look the new way. Items handed out before keep their look, but still place and level up as this type |
| `enabled: false`, or the file deleted or renamed | They make nothing until the file is back. They can still be broken |

A spawner keeps the file it was placed from: a zombie spawner placed before `zombie.yml` existed
stays a `default` spawner.

## Item spawners

`spawners/diamond.yml` ships as an example on a new server:

```yaml
spawner:
  mode: ITEM
  produces:
    spawns: DIAMOND
    name: "<#7AD7C9>A cool diamond!"
  spawn-conditions:
    players:
      range: 16
      min: 1
      max: 0
  delay: 8
```

An `ITEM` spawner makes its item, level times per cycle, into its storage or onto the ground. It has
no Auto Kill and no XP, and its menu hides those buttons. What it makes always sells as its material,
whatever its name. With Virtual Storage off, the items drop around the spawner where players can see
them. See [Storage & Selling](../features/storage.md).

## ItemsAdder and Nexo

With ItemsAdder installed, `produces.spawns` can name an ItemsAdder item in `ITEM` mode, or an
ItemsAdder mob in `ENTITY` mode, as `IA-namespace:id`. ItemsAdder mobs have no loot table to roll, so
they are spawned and killed even with modern Auto Kill.

`item-settings.material` and `block-settings.material` can be an ItemsAdder or Nexo custom block,
checked once that plugin has loaded its items. While a block is not known, because the plugin is
missing or not loaded yet, its spawners are kept, make nothing and cannot be placed, and the console
says so once until a reload. A custom block's spawner is registered a tick after it is placed.

Related: [Spawn Conditions](../features/spawn-conditions.md), [Basic Concepts](../getting-started/concepts.md), [Prices](prices.md)
