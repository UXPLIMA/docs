---
title: Enchantment files
order: 401
description: Every key of an enchantment file, item lists, conflicts, exclusive groups and routes.
icon: file-text
---

One enchantment, one file under `enchantments/`. The file name is the id: lower case letters, digits and hyphens, up to
64 characters, starting and ending with a letter or a digit. An `id` key inside the file overrides it.
`enchantments/sharpened.conf` is commented in full and is the reference the others follow.

| Key | Default | Meaning |
|---|---|---|
| `name` | The id | What a player reads; see below |
| `description` | Empty | The line the display draws under the enchantment, in the same shapes |
| `rarity` | Required | A rarity of `rarities.conf`; decides how it is found |
| `type` | Required | A type of `types.conf`; decides how it is drawn and ground off |
| `targets` | Required | One or more targets of `targets.conf`; decide what it goes on and when it fires |
| `max-level` | 1 | The highest level any route hands out; `0` or less means no ceiling |
| `overpower-max` | `max-level` | How far an anvil may push two of these past `max-level` when `anvil.overpowering` is on |
| `permission` | `uxmenchants.enchantment.<id>` | The node a player needs; `""` opens it to everyone |
| `anvil-items` | The items of its targets | What an anvil, a clicked book or `/uxmenchants apply` will put it on |
| `table-items` | The items of its targets | What the enchanting table offers it on |
| `conflicts` | `[]` | Enchantments it refuses to sit beside, from either side |
| `required` | `[]` | Enchantments that must already be on the item |
| `removed` | `[]` | Enchantments that come off when this goes on |
| `exclusive-groups` | `[]` | Only one enchantment of a group may be on an item |
| `obtaining` | See below | The routes it may be handed out through |
| `trigger` | Required | What makes it fire |
| `effects` | `[]` | What it does; see [Effects](effects.md) |
| `numbers` | None | Extra numbers for the description; see [Numbers](numbers-and-descriptions.md) |

## Name and description

Three shapes, for `name`, `description` and every other name in the content files:

```hocon
name = "@content.enchantments.sharpened.name"   # a line of messages/, in each reader's language
name { en = "Sharpened", tr = "Bilenmiş" }       # one line per language
name = "Sharpened"                               # one line everybody reads
```

The shipped files use the first; their words are under `content` in each messages file.

## Item lists

An item is written the way a material reads, in upper case. A server running a custom item plugin may name one of its
items in that plugin's own spelling, so `["DIAMOND_SWORD", "oraxen:ruby_sword"]` means both; a name no plugin answers
never matches. `@swords` names a set of `targets.conf`. Leave `anvil-items` and `table-items` out and they take the
items of the targets the enchantment names, which is right nearly every time.

## Conflicts, requirements and groups

A name with no colon is one of ours. A name with a colon belongs to another enchantment plugin and is read off the item
exactly as that plugin writes it, so `"ecoenchants:telekinesis"` refuses at the anvil and the table the same way
`"blunted"` does; a conflict against a plugin that is not installed is never met. A name in `conflicts`, `required` or
`removed` that is not defined, or that is the enchantment itself, refuses the reload.

`exclusive-groups` names a family in one word on each member. `conflicts` names one enchantment at a time, so a family
of eight is fifty-six lines; a shared group says it once. The shipped `sharpened` and `blunted` share `edge` and also
conflict, because a pair that conflicts for a reason of its own is not a family.

## obtaining

```hocon
obtaining {
  table     = true
  villager  = true
  loot      = true
  anvil     = true
  fishing   = false
  mob-drops = false
  raids     = false
}
```

`table`, `villager`, `loot` and `anvil` default to true; `fishing`, `mob-drops` and `raids` default to false, and the
matching block of `acquisition` in `config.conf` has to be on as well. The keys may also be written at the top of the
file without the block.

The anvil does not read `anvil` in this build: a book of an enchantment that writes `anvil = false` still applies. The
key shows in the wiki page and the editor.

## trigger

`attack`, `defend`, `kill`, `death`, `block-break`, `block-place`, `shoot-bow`, `projectile-hit`, `interact`, `fish`,
`move`. See [Grindstone, types and targets](grindstone-types-targets.md#triggers).

## When a file is wrong

The plugin refuses the whole reload, names the file, the key and the line, and keeps running on the content it already
had. It never loads part of a content set.

Related: [Effects](effects.md), [The shipped enchantments](catalogue.md)
