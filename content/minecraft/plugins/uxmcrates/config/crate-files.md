---
title: Crate files
order: 609
description: Every key of crates/<id>.conf, with its default and meaning.
icon: package
---

One file per crate under `crates/`; the file name is the id. `common.conf` and `elite.conf` ship as commented
examples. How the parts behave is on [Crates](../features/crates.md) and [Rewards and groups](../features/rewards.md).

## Top level

| Key | Default | Meaning |
|---|---|---|
| `name` | | The name, in one of the [three shapes](../getting-started/concepts.md#a-crate-is-a-file) |
| `key.id` | | The key file this crate takes |
| `key.amount` | `1` | Keys per opening. `0` takes no key |
| `key.kind` | | `virtual`, `physical` or `either` (items first) |
| `cost` | `[]` | Effect lines taken per opening: `[take-money] vault 500`, `[take-item] DIAMOND 3` |
| `requirements` | `[]` | `money vault >= 500`, `item DIAMOND >= 3`, `placeholder %mcmmo_level_mining% >= 50` |
| `permission` | none | A node needed beside the key |
| `shape.type` | | `block` or `menu` |
| `shape.material` | | The block, or the list icon of a menu crate |
| `shape.positions` | `[]` | `world,x,y,z` lines; `/uxmcrates place` writes them |
| `visibility.mode` | `opening.default-visibility` | `opener`, `radius` or `everyone` |
| `visibility.radius` | `opening.default-radius` | Blocks, for `radius` |
| `animation` | | A file under `animations/` |
| `idle-effect` | none | A file under `animations/` drawn while nobody opens it |
| `cooldown` | `0` | Seconds. `0` no wait, `-1` once forever |
| `worlds.mode` | `blacklist` | `blacklist` or `whitelist` |
| `worlds.list` | `[]` | World names. Empty is everywhere |
| `broadcast.enabled` | `true` | Announce wins |
| `broadcast.threshold` | `3` | The rarity `order` at or above which a win is announced |
| `effects.open` | `[]` | Effect lines when an opening is paid for |
| `effects.win` | `[]` | Effect lines once the reward is handed over. `%reward%`, `%rarity%`, `%crate%`, `%player%` |
| `no-repeat` | `true` | One opening never gives the same reward twice, while the crate has enough rewards |
| `bulk.cap` | `opening.bulk-cap` | The most openings of one `/crates massopen` on this crate |
| `bulk.announce` | `once` | `each`, `once` or `never` |
| `selection.enabled` | `false` | The player picks instead of rolling |
| `selection.picks` | `1` | How many the player takes |
| `selection.faces` | `9` | How many are shown, at most |
| `selection.face-down` | `false` | Hide the faces until taken |
| `rarities` | | Named rarities; see below |
| `groups` | | The reward table; see below |

## hologram

| Key | Default | Meaning |
|---|---|---|
| `lines` | | Lines per language. `%keys%` and `%cooldown%` are for the reader |
| `height` | `1.6` | Blocks above the block |
| `view-distance` | `32` | How far it is seen |
| `refresh-millis` | `500` | How often each viewer's text is worked out again |

`appearance` takes any of these; a key you leave out keeps the vanilla look:

| Key | Meaning |
|---|---|
| `billboard` | `center`, `vertical`, `horizontal` or `fixed`: how the text turns to the player |
| `alignment` | `center`, `left` or `right` |
| `text-shadow`, `see-through` | `true` or `false` |
| `background` | The panel behind the text, `#rrggbb` or `#aarrggbb`, or `"none"` |
| `text-opacity` | `0` to `255` |
| `glow` | The outline colour, `#rrggbb`, or `"none"` |
| `brightness-block`, `brightness-sky` | `0` to `15`, `-1` for the world's own |
| `line-width` | Where a long line wraps, in pixels |
| `line-gap` | The distance between two lines (`0.28` shipped) |
| `view-range` | The multiplier on how far the client draws the text |
| `shadow-radius`, `shadow-strength` | The shadow on the ground, `-1` for none |
| `scale` | One number, or `{ x, y, z }` |
| `offset` | `{ x, y, z }` |

## rarities

```hocon
rarities {
  epic { name = "Epic", colour = "level", order = 5, weight = 5, effects = ["[sound] ui.toast.challenge_complete 1 1"] }
}
```

| Key | Meaning |
|---|---|
| `name` | The rarity's name |
| `colour` | A role from theme.conf, never a hex code |
| `order` | Its place; the broadcast threshold compares it |
| `weight` | Optional default weight for a reward that writes none |
| `effects` | Effect lines fired after the crate's win effects when the best reward is this rarity |

## groups

```hocon
groups {
  loot {
    name = "Loot"
    rewards {
      diamond { weight = 9, display { name = "A diamond", rarity = "rare" }, items = ["DIAMOND 3"] }
    }
  }
}
```

A group has a `name` and `rewards`. Every reward key is on [Reward files](reward-files.md).

Related: [Crates](../features/crates.md), [Reward files](reward-files.md)
