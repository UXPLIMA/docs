---
title: config.conf
order: 401
description: Every key of the main file, with its shipped value and what it does.
icon: file-cog
---

The main settings file. Every key is shown with the value the plugin ships. `/uxmteleportbow reload` applies every
change except the two trail keys marked below.

## language

| Key | Default | What it does |
|---|---|---|
| `default` | `"en"` | What a player reads when nothing else answers, and the fallback for a line a translation has not reached |
| `follow-client` | `true` | A player who has chosen nothing reads the language their client is set to; `false` starts everyone in `default` |
| `force` | `""` | One language for everybody, whatever they chose and whatever their client says; empty for none |

The languages are the files in `messages/`; nothing here lists them. See [Languages](languages.md).

## item

| Key | Default | What it does |
|---|---|---|
| `bow.material` | `"BOW"` | The bow's item; keep a bow unless a resource pack re-models one |
| `bow.slot` | `1` | The inventory slot, `0` to `40`: 0 to 8 hotbar, 9 to 35 inventory, 36 to 39 armour, 40 off hand |
| `bow.lore-lines` | `9` | Lore lines shown; line N is `item.bow-lore-N` in the messages. `0` or more |
| `bow.custom-model-data` | `0` | The resource pack model; `0` for none |
| `bow.glow` | `true` | The enchanted shimmer |
| `bow.unbreakable` | `true` | The bow never wears out |
| `bow.infinite-arrows` | `true` | The shot never spends the arrow |
| `ammo.enabled` | `true` | Hand out the arrow; a bow cannot be drawn outside creative mode without one |
| `ammo.material` | `"ARROW"` | The arrow's item |
| `ammo.slot` | `9` | Its slot, `0` to `40` |
| `ammo.amount` | `1` | How many, `1` to `64` |
| `give.on-join` | `true` | Give the bow when a player joins |
| `give.on-respawn` | `true` | Give it when a player respawns |
| `give.on-world-change` | `true` | Give it when a player changes world |
| `give.respect-world-filter` | `true` | Skip the three above in a world `filters.worlds` closes |
| `locked` | `true` | The bow and the arrow cannot be moved, dragged, swapped, dropped or placed, and never drop on death |

See [The bow and the arrow](../bow/the-bow.md).

## teleport

| Key | Default | What it does |
|---|---|---|
| `cooldown-ms` | `3000` | Milliseconds between two shots; `0` turns it off |
| `fall-damage-grace-ms` | `6000` | Milliseconds after a teleport in which the first fall does no damage; `0` turns it off |
| `headroom-blocks` | `2` | Clear blocks the landing needs, counted upward; at least `1` |
| `void-floor-y` | `-40.0` | The lowest Y a player may be sent to |
| `refund-cooldown-on-refusal` | `true` | Give the plugin's cooldown back after a refusal found only after the arrow flew |
| `arrow-damage` | `0.0` | The arrow's damage; `0` or more |
| `arrow-speed-multiplier` | `1.6` | How much faster than a normal arrow it flies; more than `0` |
| `arrow-allow-pickup` | `false` | Whether a player may pick the arrow up |

See [Shooting](../bow/shooting.md).

## trail

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Draw a line behind the arrow |
| `particle` | `"END_ROD"` | The particle, with its data after a space when it needs one |
| `count` | `2` | Particles at each point |
| `offset` | `0.0` | Their spread, in blocks |
| `speed` | `0.0` | The particle's own speed |
| `tick-ms` | `100` | Milliseconds between two points. Needs a restart |
| `give-up-after-ms` | `30000` | Stop drawing an arrow never seen to land. Needs a restart |

See [Trail](../bow/trail.md).

## effects

Nine lists of action lines: `shot`, `departure`, `arrival`, `refused-cooldown`, `refused-world`,
`refused-departure`, `refused-destination`, `refused-void`, `refused-headroom`. What each ships as, and how a line is
written, is on [Effects](../bow/effects.md).

## filters

| Key | Default | What it does |
|---|---|---|
| `worlds.mode` | `"deny"` | `allow`: only a listed world works. `deny`: every world but a listed one |
| `worlds.list` | `[]` | World names |
| `regions.enabled` | `false` | Check WorldGuard regions and Towny towns |
| `regions.mode` | `"deny"` | `allow` or `deny`, as for worlds |
| `regions.check` | `"both"` | `departure`, `destination` or `both` |
| `regions.list` | `[]` | Region and town names |

See [Filters](../bow/filters.md).

Related: [Configuration](index.md), [Messages](messages.md)
