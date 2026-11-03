---
title: Tracks
order: 401
description: What a track is, its file, the level curve, the shipped three tracks, and adding, renaming or removing one.
icon: route
---

A track is a file in `tracks/`. The file name without `.conf` is the track's id: 1 to 32 lower-case letters, digits
and hyphens, starting and ending with a letter or digit. The id is what events, commands, placeholders and boards
name; it never contains an underscore, which is what lets a placeholder such as `%uxmlevels_mining_level%` split.

```hocon
display-name = "@track.mining.name"
icon = "DIAMOND_PICKAXE"

maximum-level = 100
base-cost = 150
growth = 1.14

hud {
  experience-bar = false
  boss-bar = true
  action-bar = false
}

rewards {
  efficient { ... }
}
```

## The keys

| Key | Default | What it does |
|---|---|---|
| `display-name` | the id | The name players read: your own MiniMessage, or `@key` for a line of the messages files in each player's language |
| `icon` | `"EXPERIENCE_BOTTLE"` | The material of the track's tile in the windows |
| `maximum-level` | `100` | The top of the ladder, `2` to `10000` |
| `base-cost` | `100` | Experience from level 1 to 2; at least `1` |
| `growth` | `1.15` | What each level costs over the one before; at least `1.0` |
| `hud.experience-bar`, `hud.boss-bar`, `hud.action-bar` | `false` | Which heads-up surface shows this track. See [Heads-up](heads-up.md) |
| `rewards { }` | | The rewards of the track. See [Rewards](rewards.md) |

## The curve

Level 1 to 2 costs `base-cost`; every level after costs the one before times `growth`, rounded, never less than 1.
`growth = 1.0` makes every level cost the same. A curve that would overflow stops the load with a message naming the
level.

Only the experience is stored; the level is read from it through the curve. Lowering `maximum-level` or raising the
costs lowers every player's level at the next read, and nothing has to be migrated. At the top level a track takes no
more experience.

The shipped curves grow fast:

| Level | `global`: 100 and 1.15 | `mining`, `combat`: 150 and 1.14 |
|---|---|---|
| 1 to 2 | 100 | 150 |
| 2 to 3 | 115 | 171 |
| 10 to 11 | 352 | 486 |
| 25 to 26 | 2,860 | 3,468 |
| 50 to 51 | about 94,000 | about 92,000 |
| 99 to 100 | about 89 million | about 56 million |
| Level 1 to 100 in all | about 680 million | about 459 million |

The comment in `tracks/global.conf` says the last level costs about a hundred thousand; it costs about 89 million.
For a ladder players can finish, lower `growth`, such as `1.05`, or the `maximum-level`.

## The shipped tracks

| Track | Name | Icon | Shows on | Rewards |
|---|---|---|---|---|
| `global` | Global | `EXPERIENCE_BOTTLE` | The experience bar | `starter` at 5, `settled` at 25, `veteran` at 50 (automatic) |
| `mining` | Mining | `DIAMOND_PICKAXE` | The boss bar | `efficient` at 20 |
| `combat` | Combat | `IRON_SWORD` | The action bar | `blooded` at 15 |

## Adding, renaming, removing

| You do | What happens |
|---|---|
| Add a file | A new track after `/uxmlevels reload`. It pays nothing until an event names it |
| Rename a file | The track has a new id. Events that name the old id pay nothing, and the console names each. Players' experience stays under the old id in the database |
| Delete a file | The track is gone from the windows, commands and placeholders. Its rows stay in the database, and come back if the file does |

The tracks are listed in file name order, in the windows and on `/levels level`.

## Earning on a track

A player earns on a track unless their permissions say otherwise: `uxmlevels.track.<id>` set to `false` keeps them
off that one track, `uxmlevels.track.*` set to `false` off all of them. An unset node earns. The nodes are read when
the player joins. See [Permissions](../permissions/reference.md#tracks).

Related: [Events](events.md), [Track files](../config/track-files.md)
