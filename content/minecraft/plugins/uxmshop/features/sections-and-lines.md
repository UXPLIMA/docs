---
title: Sections and lines
order: 301
description: Section keys, line keys and types, item blocks, rotation, refusals, and the shipped sections.
icon: layout-list
---

A section is a file under `shops/`. Its file name is its id, the argument of `/shop open <section>`. A section is open
to everybody unless its `permission` names a node; `uxmshop.section.<section>` is the usual name for that node, and the
hub, `/shop open` and a shortcut line all ask the same one. Each file holds the section's settings and a `lines` block,
one entry per product.
Every shipped section explains the grammar in a comment at the top.

## Section keys

| Key | Meaning |
|---|---|
| `order` | Where the section sits on the hub; HOCON does not keep the order a block was written in |
| `menu` | Which window in `menus/` draws it; `section` by default |
| `permission` | The node needed to open it; left out, everybody may |
| `buy-permission` | The node needed to buy from it |
| `sell-permission` | The node needed to sell to it; a section with only a buy node is bought from and not sold to |
| `currency` | Which currency its lines price in; left out, `shop.default-currency` |
| `icon` | The tile the hub shows |
| `name`, `description` | A plain string for everybody, or a block per language: `name { any = "Tools", tr = "Aletler" }` |
| `band` | The [dynamic price band](prices.md#dynamic-band) every line takes unless it names its own |
| `rotation` | `enabled`, `size` (6), `every` (`24h`): shows `size` lines at a time and swaps them every `every` |

A rotation is worked out from the clock, so every server behind one proxy shows the same lines.

## Line keys

| Key | Meaning |
|---|---|
| `order` | Where the line sits on the page |
| `type` | `item`, `command`, `shortcut` (to another section, named by `section`), `decoration` or `blank` |
| `item` | What an item line hands over |
| `icon` | The face of the tile, when it differs from what is sold |
| `buy` | What a player pays; left out, the line cannot be bought |
| `sell` | What a player is paid; left out, it cannot be sold |
| `match` | How strictly a sold item is [matched](item-matching.md) |
| `permission` | The node needed for this line |
| `conditions` | What a player must meet, as `"%placeholder% >= 10"` |
| `actions` | What runs after a purchase |
| `floor`, `ceiling` | The lowest and highest the price may reach |
| `band` | A band for this line alone |
| `stock` | [Server stock and per player quotas](stock-and-quotas.md) |
| `currency` | The currency of this line |
| `name`, `description` | The line's own words |

An `item` block reads `material`, `amount`, and any of `name`, `lore`, `enchants`, `stored-enchants`,
`custom-model-data`, `damage`, `unbreakable`, `potion`, `potion-effects`, `colour`, `firework`, `trim`, `head`,
`spawner`, `patterns`, `model`. `/uxmshop iteminfo` prints the block of the item in your hand.

A line's `actions` run without the message files: write plain MiniMessage, not a theme tag or an `@` key.

## What is refused

- A `command` line that can be sold back.
- A line that pays more than it charges.
- An item field the plugin does not know, so a typo is never mistaken for a setting that worked.

The console lists every problem at start and on reload; the rest of the catalogue loads.

## The shipped sections

Both price in `experience`, so a bare server trades.

| Section | Line | Buy | Sell | Match |
|---|---|---|---|---|
| `blocks` | 64 stone | 64 | 16 | `material` |
| `blocks` | 64 dirt | 32 | 8 | `material` |
| `blocks` | 64 glass | 128 | 32 | `material` |
| `tools` | Iron pickaxe | 350 | 90 | `full-meta-ignoring:damage` |
| `tools` | 64 cobblestone | 32 | 8 | `material` |
| `tools` | Miner's Haste (a command line) | 500 | | |

Miner's Haste runs `[effect] haste 600 2` and `[message] @content.haste-given`: Haste II for ten minutes, on Paper and
Folia alike. It is the example of a line that sells something other than an item.

Related: [Section files](../config/section-files.md)
