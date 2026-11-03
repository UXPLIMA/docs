---
title: Plot worlds
order: 301
description: Every key of a world file, from the grid, layers, walls, roads, edits, pre-generation, game mode, flags, entities, price and looks.
icon: globe
---

One plot world per file under `worlds/`; the file name is the world name. Copy the file for a second world: a beginners'
world of small plots beside a builders' world of large ones is two files.

## The grid

| Key | Default | Meaning |
|---|---|---|
| `create-world` | true | Create the world on start when the server does not have it; on Folia it goes into `bukkit.yml` instead |
| `plot-size` | 64 | Plot width in blocks |
| `road-size` | 7 | Road width; `0` puts plots against each other |
| `origin.x`, `origin.z` | 0, 0 | Shift the whole grid, to keep spawn on a road |
| `bottom-y` | -64 | The lowest block |
| `layers` | `bedrock 1`, `dirt 2`, `grass_block 1` | The ground from the bottom up; the last is the plot floor |
| `walls.body`, `unclaimed-top`, `claimed-top` | smooth stone, stone slab, brick slab | The ring; the top tells passers-by which plots are free |
| `road-material` | `smooth_stone_slab` | The road surface |
| `biome` | `plains` | Painted over the whole world |

These are written by the generator. Changing them after the world exists is refused.

## Rules

| Key | Default | Meaning |
|---|---|---|
| `max-plots-per-player` | 0 | This world's own limit; 0 leaves only the total |
| `visit-policy` | `open` | What a new plot starts as: `open`, `trusted` or `closed` |
| `guard-edits` | true | Hold WorldEdit and FastAsyncWorldEdit edits inside the plot a player may build on |
| `flags` | `{}` | Flag defaults you disagree with; see [Flags](flags.md) |
| `locked-flags` | `[]` | Flags no plot here may change |
| `starter-template` | `""` | A layout from `templates/` every new plot starts with; the plot is photographed first |

## Edits

With `guard-edits`, every block a WorldEdit command would write is asked about first, and one that lands outside the
plot is not written; the rest of the edit happens. FastAsyncWorldEdit is told which plot a player may edit, so its own
region restrictions can stay on; it asks for `fawe.uxmplots`, which everybody holds unless you take it away.
`uxmplots.bypass` is not held to it.

## pre-generation

| Key | Default | Meaning |
|---|---|---|
| `radius-chunks` | 0 (off) | How far out the square reaches; 100 is 40401 chunks and takes hours |
| `chunks-per-batch` | 4 | Chunks asked for each second |
| `tps-floor` | 18.0 | Waits below this; never below 5 |

Walked from the middle out, and its progress is written into the world folder, so a restart continues.
`/uxmplots pregen stop` and `start` pause it until a restart.

## game-mode

`mode` (`""` for none, or `survival`, `creative`, `adventure`, `spectator`) puts everybody walking in into that mode and
back to the server default on the way out. `on-entering` and `on-leaving` are effect lists. `uxmplots.gamemode.keep`
is left alone. The inventory is never touched.

## entities

| Key | Meaning |
|---|---|
| `default` | How many of one kind a plot may hold when `per-type` does not name it |
| `total` | How many of everything |
| `per-type` | `MINECART = 8`, `ARMOR_STAND = 24`, `VILLAGER = 4` |

Zero is no limit, which ships. Nobody builds a lag machine out of blocks; they build it out of four hundred minecarts.

## price

| Key | Meaning |
|---|---|
| `first` | What the first plot costs |
| `step` | What each further plot adds (`linear`) or multiplies by (`geometric`) |
| `curve` | `flat`, `linear` or `geometric`; a geometric step below 1 is refused and the price stays flat |
| `currency` | An id from `currencies.conf`; none is the first one |
| `refund-share` | How much comes back on giving a plot up, 0 to 1, at the price of the plot handed back |

Leave the block out and plots are free, which ships. Counted in this world alone.

## patterns and biomes

What an owner may lay with `/plot floor`, `wall`, `border` and `ground`:

```hocon
patterns {
  floor {
    gold-veined {
      materials = [ { material = "smooth_stone", weight = 3 }, { material = "gold_block", weight = 1 } ]
      tile = 2
      permission = "uxmplots.floor.gold-veined"
    }
  }
}
```

A pattern is a list with weights, walked by position, so a mix is a weave and laying it twice looks the same. `tile`
is how wide one square is. An empty `permission` gives it to every owner here; a node can also be bought in the
[shop](personality-and-shop.md). The shipped world offers floors `plain`, `chequer`, `gold-veined`; walls `plain`,
`banded`; grounds `plain`, `stone`; borders `plain`, `quartz`.

`biomes` maps a word to a biome: `plains`, `forest`, and `snowy` (`minecraft:snowy_taiga`, behind
`uxmplots.biome.snowy`). A biome writes no block, so nothing is photographed.

## The world editor

`/uxmplots world <world>` opens twelve buttons for the rules above; see [In-game editors](editors.md).

Related: [Claiming](claiming.md), [Shaping a plot](shaping.md)
