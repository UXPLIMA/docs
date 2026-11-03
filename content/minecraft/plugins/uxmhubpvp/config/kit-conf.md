---
title: kit.conf
order: 402
description: The kit a fighter is handed, every piece, and every key an item takes.
icon: shield-half
---

`kit.conf` is the kit a player is handed when they turn PvP on. It is written once, at the first start, and an update
never merges it: a piece you delete stays deleted. `/uxmhubpvp reload` reads it again.

```hocon
kit {
  weapon-slot = 0

  helmet     { material = "IRON_HELMET",     unbreakable = true, flags = ["HIDE_ATTRIBUTES", "HIDE_UNBREAKABLE"] }
  chestplate { material = "IRON_CHESTPLATE", unbreakable = true, flags = ["HIDE_ATTRIBUTES", "HIDE_UNBREAKABLE"] }
  leggings   { material = "IRON_LEGGINGS",   unbreakable = true, flags = ["HIDE_ATTRIBUTES", "HIDE_UNBREAKABLE"] }
  boots      { material = "IRON_BOOTS",      unbreakable = true, flags = ["HIDE_ATTRIBUTES", "HIDE_UNBREAKABLE"] }
  weapon {
    material = "DIAMOND_SWORD"
    name = "@item.blade"
    enchants { sharpness = 1 }
    unbreakable = true
    flags = ["HIDE_ATTRIBUTES", "HIDE_UNBREAKABLE"]
  }
  off-hand   { material = "SHIELD", unbreakable = true, flags = ["HIDE_ATTRIBUTES", "HIDE_UNBREAKABLE"] }

  extras {
    1 { material = "GOLDEN_APPLE", amount = 5 }
    2 { material = "COOKED_BEEF",  amount = 16 }
  }
}
```

## The pieces

| Key | Goes to |
|---|---|
| `weapon-slot` | The hotbar slot the weapon goes into, `0` to `8`, and the slot the player holds. Default `0` |
| `helmet`, `chestplate`, `leggings`, `boots` | The armour slots |
| `weapon` | `weapon-slot` |
| `off-hand` | The off hand |
| `extras` | Any other items, each keyed by its inventory slot: `0` to `8` is the hotbar, `9` to `35` the rest |

A piece that is missing, or has no `material`, is left out: comment out a material to switch a piece off without
losing it. An extra in the weapon's slot replaces the weapon, since the extras go in last; keep them apart. The leave face of the toggle item goes into `toggle-item.in-pvp.slot`, 8 as shipped,
on top of everything.

## An item

| Key | Default | Meaning |
|---|---|---|
| `material` | none | The item, such as `DIAMOND_SWORD`. An unknown name leaves the piece out and the console says so |
| `amount` | `1` | The stack size |
| `name` | none | `@key` for a line of the messages files, read in each player's language, or your own MiniMessage |
| `lore` | none | A list of lines, each `@key` or your own words |
| `enchants` | none | `{ sharpness = 1, unbreaking = 3 }`: the enchantment's key and its level |
| `flags` | none | Item flags, such as `HIDE_ATTRIBUTES`, `HIDE_ENCHANTS`, `HIDE_UNBREAKABLE` |
| `unbreakable` | `false` | The item never wears out |
| `glow` | `false` | The enchantment shine, without an enchantment |
| `hide-vanilla-tooltip` | `false` | Hides the lines the client writes under the lore, such as armour values |
| `custom-model-data` | none | A resource pack's model number |

An unknown enchantment or flag is left off the item, and the console names it.

## Examples

A bow fight, with arrows in the last hotbar slot but one:

```hocon
kit {
  weapon-slot = 0
  weapon { material = "BOW", enchants { power = 2, infinity = 1 }, unbreakable = true }
  extras {
    1 { material = "STONE_SWORD", unbreakable = true }
    7 { material = "ARROW", amount = 1 }
  }
}
```

A fist fight: delete every piece and leave `extras` empty. The fighter holds nothing but the leave face.

Related: [The kit](../fighting/kit.md), [config.conf](config-conf.md#toggle-item)
