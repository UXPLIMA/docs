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

A spawner type can use custom content from ItemsAdder or Nexo. Names carry the plugin in front:
`IA-namespace:id` for ItemsAdder, `NEXO-id` for Nexo, the prefix in any case.

| Key | ItemsAdder | Nexo |
|---|---|---|
| `spawner.produces.spawns`, `ENTITY` mode | A custom mob | No |
| `spawner.produces.spawns`, `ITEM` mode | A custom item | No |
| `item-settings.material` | A custom block's item | A custom block's item |
| `block-settings.material` | A custom block | A custom block, but not a string block |

### Custom mobs

```yaml
spawner:
  mode: ENTITY
  produces:
    spawns: "IA-myserver:demon"
```

An ItemsAdder mob has no loot table to roll, so even with modern Auto Kill it is spawned and killed,
as a kill of an online player, and pays what it drops, times the spawner's level. A mob that
survives its kill is removed. Without Auto Kill it is a living mob like any other, and players kill
it for its drops. Loot table files do not apply to ItemsAdder mobs, and in a Peaceful world their
spawners wait.

`/usp give` takes your file names, not ItemsAdder ids, so make one file per custom mob, such as
`spawners/demon.yml`. Its spawner's name shows the mob through `<entity_name_lang>`: give it a name in
`translates` of the language file under its full name in capitals, `"IA-MYSERVER:DEMON": "Demon"`.

### Custom items

```yaml
spawner:
  mode: ITEM
  produces:
    spawns: "IA-myserver:ruby"
    name: ""
    lore: []
```

The spawner makes that ItemsAdder item. `produces.name` and `lore` replace the item's own when they
are set; leave them empty to keep the ItemsAdder look. What an `ITEM` spawner makes sells as its
material, so a ruby made on paper sells at `PAPER`'s price in `default-prices`, or the shop's price
for paper. See [Prices](prices.md).

### Custom blocks

```yaml
item-settings:
  material: "NEXO-ruby_spawner"
  name: "<#EE8C8C><bold>Ruby Spawner"

block-settings:
  material: "NEXO-ruby_spawner"
```

`item-settings.material` makes the spawner item from the custom block's item: the name, lore,
enchantments, glint and `model` of `item-settings` are put on top of it. Placing it, ItemsAdder or
Nexo places its own block, and uxmSpawners registers the spawner a tick later, as the block
`block-settings` names.

`block-settings.material` is what the placed spawner is. `show-inside` has no effect on a custom
block. The two settings are apart: an ItemsAdder item can place a plain `SPAWNER`, and a plain
spawner item can place a custom block.

A custom block spawner works like any other:

| What | How |
|---|---|
| Right-click | Opens its menu |
| Breaking | uxmSpawners breaks it by its own rules and drops the spawner item. A Nexo block does not drop its own item |
| Explosions | Kept out of them while `protect-spawners-from-explosions` is on. Otherwise it is removed without dropping the custom block's item, and its storage is handled as `lost-spawner-storage` says |
| Claim plugins | Asked about it as about a spawner |
| Changing `block-settings` | Placed spawners turn into the new block, as with any block |

A spawner of a custom block is registered a tick after it is placed. If it is broken, blown up or the
server stops in that tick, the custom block stays without a spawner and the item is used up.

### When the plugin is missing or not ready

ItemsAdder and Nexo load their items after the server starts. uxmSpawners checks the names once
the plugin says its items are loaded, and on every `/usp reload` after that.

| The name in the file | Result |
|---|---|
| `item-settings.material` of a plugin that is not installed, or no block of it | The item is a plain `SPAWNER`, with a warning |
| `block-settings.material` of a plugin that is not installed or not loaded yet, or no block of it | Its spawners are kept and make nothing, and none can be placed. Their menu says the block is not known, and the console says so once until a reload. When the plugin finishes loading, they get their block and work |
| `block-settings.material` that is a Nexo string block | A plain `SPAWNER` is used, with a warning: water and a missing block under it would remove it |
| `produces.spawns` that is no ItemsAdder item or mob | A warning with the file name; its spawners make nothing |

A spawner whose custom block is not known is never deleted as a lost spawner. An ItemsAdder version
whose API does not fit stops uxmSpawners at startup, naming it.

Related: [Spawn Conditions](../features/spawn-conditions.md), [Basic Concepts](../getting-started/concepts.md), [Prices](prices.md)
