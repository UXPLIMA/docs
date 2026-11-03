---
title: Drawing
order: 311
description: Packet minions, view distance, the hologram, talk lines, animation and the tick.
icon: eye
---

## render

| Key | Default | Meaning |
|---|---|---|
| `view-distance` | 48 | How far away a player is sent a minion, in blocks |
| `refresh-ticks` | 40 | How often the drawn minion is refreshed |
| `menu-refresh-ticks` | 20 | How often an open window's numbers are read again |
| `animation` | true | The arm swing while it works; the packet cost on a busy server |
| `talk` | true | The lines a minion says, under the hologram |
| `talk-interval-ticks` | 600 | Ticks between two lines, at the soonest |
| `talk-lines` | `@minion.talk.one` to `four` | One is picked at random |

## The hologram

`hologram.conf`: drawn through uxmLib, so no hologram plugin is needed, and nothing is spawned.

| Key | Default |
|---|---|
| `enabled` | true |
| `offset-y` | 2.2 blocks |
| `view-distance` | 24 |
| `update-interval-ticks` | 20 |
| `lines` | `@minion.hologram.name`, `level`, `status`, `storage` |

`appearance` takes the suite's shared hologram table: `billboard` (`center`, `vertical`, `horizontal`, `fixed`),
`alignment`, `text-shadow`, `see-through`, `background` (`#aarrggbb` or `none`), `text-opacity`, `glow`,
`brightness-block`, `brightness-sky`, `shadow-radius`, `shadow-strength`, `line-width`, `view-range`, `line-gap`,
`scale`, `translation`. A key left out keeps the vanilla look. With the hologram off there is no talk line either.

## The tick

```hocon
tick {
  interval-ticks       = 20
  max-actions-per-pass = 0
}
```

Minions are indexed by chunk. One pass reads the index off the main thread, skips unloaded chunks and offline owners,
and posts the due minions to the region that owns their block. `max-actions-per-pass` (0 for all) only spreads a huge
pass over the next ones; every due minion still runs. There is no threads per minion setting: the threads are the
platform's.

## placeholders

`placeholders.refresh-ticks` (40): how often the per player placeholder numbers are read. A placeholder never reads the
database on the call.

Related: [hologram.conf](../config/hologram-conf.md)
