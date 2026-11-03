---
title: Personality and shop
order: 309
description: Music, ambience, time, weather and sign looks for a plot, and the shop that sells looks and patterns.
icon: sparkle
---

## Personality

`/plot feel <kind> [choice]` sets what a plot sounds and looks like to the players standing in it; the world's own
time, weather and sound go on unchanged. `catalogues/personality.conf` offers:

| Kind | Shipped |
|---|---|
| `music` | `cat`, `blocks`, `far`, `cave` (ambient cave, looped every 13 s), `pigstep` (`uxmplots.music.pigstep`), `otherside` (16 diamonds) |
| `ambience` | `fireflies`, `ash`, `snow`, `sparkle`, `hearts` (8 diamonds) |
| `time` | `dawn`, `noon`, `dusk`, `midnight` |
| `weather` | `clear`, `rain`, `storm` |
| `hologram` | `billboard` (8 diamonds), `plain`, `far` (12 diamonds): how the plot sign is drawn |

Each entry has a `value`, a `word`, an `icon`, and optionally a `permission` and a `price`. `personality` in
`config.conf`: `ambience-particles` (3 per player per pass) and `sign-radius-plots` (2: the plot and its ring).

## The shop

`/plot shop` opens the [shop](../menus/shop.md). `catalogues/shop.conf` prices the faces a world offers:

| Kind | Shipped |
|---|---|
| `floor` | `gold-veined`, 24 diamonds, or `uxmplots.floor.gold-veined` |
| `wall` | `banded`, 12 diamonds |
| `border` | `quartz`, 12 diamonds |
| `ground` | `stone`, 8 diamonds |
| `tag` | nothing: a badge is sold only when `tags.conf` gates it |

Each entry: `word`, `icon`, `permission` (grants it free) and `price` (`currency`, `amount`). The name must match the
world file's. A node and a price are two roads to one thing: write both and a rank gets it free while everybody else
buys it. Something bought is bought once and used on every plot the player will ever hold, for good.

`/plot buy <kind> <entry>` buys without the window; `/plot owned` lists what you have.

Related: [catalogues](../config/catalogues.md), [Levels](levels.md)
