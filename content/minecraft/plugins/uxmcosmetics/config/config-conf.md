---
title: config.conf
order: 701
description: Every block and key of config.conf, with its default and what it does.
icon: file-cog
---

## language

| Key | Default | Meaning |
|---|---|---|
| `default` | `"en"` | What a player reads when nothing else answers |
| `follow-client` | `true` | A player who chose nothing reads their client's language |
| `force` | `""` | One language for everybody. Empty for none |

## storage

| Key | Default | Meaning |
|---|---|---|
| `type` | `"sqlite"` | `sqlite`, `mysql`, `mariadb` or `postgresql`. Restart to change |
| `file` | `"cosmetics.db"` | The SQLite file, inside `data/` |
| `host`, `port` | `"localhost"`, `0` | The database server; port `0` is 3306 or 5432 |
| `database`, `username`, `password` | `"minecraft"`, `"minecraft"`, `""` | The database and its user |
| `table-prefix` | `"uxmcosmetics_"` | Written in front of every table |
| `pool-size` | `4` | Connections; SQLite uses one |
| `sync-seconds` | `2` | How often this server reads what other servers changed for players here. `0` off |
| `local-only` | `false` | This server keeps its rows in its own SQLite file |
| `copy-from-file` | `false` | Copies the SQLite rows into the empty tables of the new database once |

## permissions

| Key | Default | Meaning |
|---|---|---|
| `wildcard` | `true` | `uxmcosmetics.*` alone gives every piece and box. Off: each needs its own node. Restart to change |

## animation

| Key | Default | Meaning |
|---|---|---|
| `tick-ms` | `100` | How often the one shared task runs. Every particle, follower and station is driven from it. Raise it on a busy server. Restart to change |
| `follower-speed` | `0.35` | The share of the distance a follower travels each frame; 1.0 snaps |
| `max-particles-per-frame` | `64` | The most particle points one player may cost per frame |

## economy

| Key | Default | Meaning |
|---|---|---|
| `purchase-enabled` | `true` | Pieces can be bought. Off: the wardrobe still works |
| `currency` | `"exp"` | `exp`, `levels`, `vault`, `vault-unlocked`, `playerpoints`, `ecobits:<currency>` or `treasury[:<currency>]` |
| `format` | `"#,##0.## 'exp'"` | A Java number pattern; text in single quotes is literal: `"'$'#,##0.00"` writes `$1,250.00` |
| `treasury-wait-ms` | `250` | How long one Treasury call may take |
| `duplicate-refund-fraction` | `0.25` | What a box duplicate pays back, as a share of the piece's price |

## opener

| Key | Default | Meaning |
|---|---|---|
| `enabled` | `true` | Hand out the wardrobe opener |
| `give-on-join` | `true` | At every join |
| `slot` | `8` | Hotbar slot, 0 to 8 |
| `material` | `"NETHER_STAR"` | The item |
| `keep-on-death` | `true` | Handed back after a respawn |
| `colour` | `"3"` | The title colour: a wheel place, a role or a gradient |

## gadget

| Key | Default | Meaning |
|---|---|---|
| `slot` | `5` | The hotbar slot a worn gadget takes |

## cleanup

| Key | Default | Meaning |
|---|---|---|
| `sweep-on-start` | `true` | Removes followers left behind by a crash, at start |

## menu

| Key | Default | Meaning |
|---|---|---|
| `sounds.click`, `page`, `denied` | pling, page turn, bass | `{ name, volume, pitch }`; an empty name silences one |
| `colours.categories` | trail 0, halo 1, cloak 2, hat 3, suit 4, emote 5, balloon 6, gadget 7, pet 8, mount 9, morph 10, banner 11 | Each family's colour: a wheel place, a role or a gradient |
| `colours.tiers` | common muted, uncommon good, rare info, epic rank, legendary cta | Each box tier's colour |
| `roulette.spins` | `26` | Steps of the strip |
| `roulette.start-interval-ms`, `end-interval-ms` | `60`, `400` | Its speed at the start and the end |
| `roulette.tick-sound`, `win-sound` | `minecraft:ui.button.click`, `minecraft:entity.player.levelup` | Its sounds |

## station

What a new station starts with. A station already placed keeps its own values in `stations.conf`.

| Key | Default | Meaning |
|---|---|---|
| `radius` | `1.5` | How far from the block a click counts |
| `clicks` | `["right"]` | `right`, `left` or both |
| `effect.particle` | `"ELECTRIC_SPARK"` | The figure's particle. Pick one that stays where it is put |
| `effect.pattern` | `"HALO"` | `HALO`, `ORBIT`, `HELIX` or `TRAIL` |
| `effect.radius`, `height` | `0.6`, `0.75` | The figure's size and height above the block |
| `effect.period-ms` | `150` | How often it is drawn |
| `effect.sound` | `"minecraft:block.ender_chest.open"` | For the player who used it; empty for none |
| `effect.use-particle` | `"TOTEM_OF_UNDYING"` | A burst for everyone near; `"dust #ffaa00"` for one with data; empty for none |
| `hologram.height` | `1.6` | The sign above the block |
| `hologram.lines` | `["<h:'{box}'>", "@station.sign.open"]` | `[]` for no sign |
| `hologram.appearance` | | `billboard` (CENTER), `alignment` (CENTER), `seeThrough` (false), `textShadow` (true), `background` (`transparent`, `none` or a colour), `glow` (`none`), `lineWidth` (200), `viewRange` (1.0), `scale` (1.0) |

## effects

What each moment does, as action lines. Write `[]` for silence; delete a list to keep the shipped one.

| Moment | Shipped |
|---|---|
| `equipped` | `[message] @equip.done`, `[sound] block.amethyst_block_chime 0.6 1.4` |
| `unequipped` | `[message] @unequip.done` |
| `unequipped-all` | `[message] @unequip.all` |
| `bought` | `[message] @buy.done`, `[sound] entity.player.levelup 0.7 1.2` |
| `box-won` | `[message] @box.won`, `[sound] entity.player.levelup 0.7 1.4` |

### Action lines

| Verb | Payload |
|---|---|
| `[message]` | A line to the player; `@key` names a messages line |
| `[broadcast]` | A line to everyone |
| `[title]` | Across the middle of the screen |
| `[subtitle]` | `<title> \| <subtitle> <fade-in> <stay> <fade-out>`, times in seconds |
| `[actionbar]` | Over the hotbar |
| `[sound]` | `<name> <volume> <pitch>` |
| `[particle]` | `<name> <count> <spread> [data]`; data is a colour, a block or an item |
| `[console]` | A command run by the server |
| `[player]` | A command run by the player |

Related: [Category files](category-files.md), [boxes.conf](boxes-conf.md)
