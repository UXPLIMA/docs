---
title: config.conf
order: 601
description: Every key of the main file, with its shipped value and what it does.
icon: file-cog
---

The main settings file. Every key is shown with the value the plugin ships. `/uxmkotl reload` applies a change
unless the key says otherwise. The wording of every line, the stick's name and the window titles included, is not
here but in the [messages files](messages.md).

## language

| Key | Default | What it does |
|---|---|---|
| `default` | `"en"` | What a player reads when nothing else answers, and the fallback for a line a translation has not reached |
| `follow-client` | `true` | A player who has chosen nothing reads the language their client is set to; `false` starts everyone in `default` |
| `force` | `""` | One language for everybody, whatever they chose and whatever their client says; empty for none |

The languages are the files in `messages/`; nothing here lists them. See [Languages](languages.md).

## storage

| Key | Default | What it does |
|---|---|---|
| `type` | `"sqlite"` | `sqlite`, `mysql`, `mariadb` or `postgresql` |
| `file` | `"kotl.db"` | The SQLite file, inside `data/`. Read only for `sqlite` |
| `host` | `"localhost"` | The database server, for the other three types |
| `port` | `0` | Its port; `0` is the type's own, 3306 for MySQL and MariaDB, 5432 for PostgreSQL |
| `database` | `"minecraft"` | The database name |
| `username` | `"minecraft"` | The user |
| `password` | `""` | The password |
| `table-prefix` | `"uxmkotl_"` | Written in front of every table, so two plugins, or two copies of one, can share a database. Letters, digits and underscores |
| `pool-size` | `4` | How many connections the pool may open; SQLite always uses one |
| `sync-seconds` | `2` | How often this server reads what other servers on the same database changed for a player who is here: their claims, kills and time held; `0` turns it off. SQLite reads nothing |
| `local-only` | `false` | This one server keeps its rows in the SQLite file whatever `type` says, for a server of a network that has no business in the shared database |
| `copy-from-file` | `false` | Copies the rows of the SQLite file into the empty tables of the new database once, at the start |

The storage is opened once: a change needs a restart. See [Database](../database/index.md) for each backend and
for moving between them.

## crown

| Key | Default | What it does |
|---|---|---|
| `claim-cooldown-ms` | `1500` | How long a new king is protected from another claim, in milliseconds. Not negative. A free plate is always claimed at once |
| `ring.enabled` | `true` | Draws a ring of particles over the king's head; `false` draws none |
| `ring.colour` | `"#FFE66D"` | Its colour, as `#rrggbb` |
| `ring.radius` | `0.32` | Its radius in blocks. About half a head; above `0.5` it reads as a circle on the floor |
| `ring.height` | `0.45` | How far above the eyes it is drawn |
| `ring.points` | `12` | How many particles make it, at least 3 |
| `ring.size` | `0.7` | How large one particle is, above 0 |
| `ring.period-ms` | `250` | How often it is drawn again, above 0 |

See [The crown](../arenas/crown.md).

## boundary

| Key | Default | What it does |
|---|---|---|
| `outline.enabled` | `true` | Draws the box of the arena an operator has open in the editor, for that operator alone; `false` draws none |
| `outline.colour` | `"#48CAE4"` | Its colour |
| `outline.spacing` | `1.5` | Blocks between two particles on an edge. Smaller draws a solid line and costs more packets. The outline never has more than 600 points |
| `outline.period-ms` | `1000` | How often it is drawn again |

The comment above this block in the shipped file speaks of `pos1` and `pos2`; there are no such commands. The outline
follows the editor. See [Arenas](../arenas/overview.md#the-outline).

## plate

| Key | Default | What it does |
|---|---|---|
| `repel.enabled` | `false` | Pushes a player whose claim was refused off the plate |
| `repel.strength` | `0.6` | How hard, sideways, away from the middle of the plate. Not negative |
| `repel.upward` | `0.35` | How hard upward |

## stick

| Key | Default | What it does |
|---|---|---|
| `material` | `"STICK"` | The item every player gets on entry. An unknown name gives a stick |
| `slot` | `0` | The hotbar slot, `0` to `8`. An item already there stays, and the stick takes the first free slot |
| `knockback-level` | `2` | The Knockback level; `0` gives no enchantment. Not negative |

See [Fights](../arenas/fights.md#the-knockback-stick).

## rewards

| Key | Default | What it does |
|---|---|---|
| `claim.money` | `[]` | Money lines paid to a player who takes the crown, such as `"vault 10"` |
| `claim.commands` | `[]` | Console commands run for them; `%player%` is their name |
| `kill.money` | `[]` | Money lines paid to a player credited with a kill |
| `kill.commands` | `[]` | Console commands run for them |

A money line that cannot be read stops the reload and names the setting. See [Rewards](../arenas/rewards.md).

## economy

| Key | Default | What it does |
|---|---|---|
| `treasury-wait-ms` | `250` | How long one Treasury call may take before the payment is given up on. Above 0. Every other economy ignores it |

## leaderboard

| Key | Default | What it does |
|---|---|---|
| `size` | `10` | How many places each board keeps; a place past this names nobody |
| `refresh-seconds` | `15` | How often the boards are read from the database. A reign, claim or kill shows at the next read. Needs a restart |
| `empty-head` | `"entity/player/wide/steve"` | What `%uxmkotl_top_<board>_<period>_<n>_head%` gives for a place nobody holds: a texture path, a player's name or id |
| `periods` | `[ alltime, monthly, weekly, daily ]` | The periods each board is kept for; an empty list keeps all four |
| `time-zone` | `""` | The zone a day starts in, such as `"Europe/Istanbul"`; empty is the server's. Servers sharing a database should write the same |
| `week-starts` | `monday` | The day a week starts on |
| `keep-windows` | `12` | How many ended windows of a period are kept, so last week's winner can still be read; older ones are deleted |

The boards are `held`, `longest`, `claims` and `kills`. See [Placeholders](../placeholders/reference.md#boards).

## menu

| Key | Default | What it does |
|---|---|---|
| `sounds.click` | `block.note_block.pling`, volume `0.6`, pitch `1.5` | A button that does something, unless its own actions play a sound |
| `sounds.page` | `item.book.page_turn`, `0.7`, `1.0` | A page arrow |
| `sounds.denied` | `block.note_block.bass`, `0.6`, `0.9` | A button the player may not use |
| `top.size` | `10` | How many players the [leaderboard window](../menus/top.md) is built from, `1` to `54` |

A sound is written `{ name = "...", volume = 0.6, pitch = 1.5 }`, by its vanilla key; an empty name silences it.

## effects

The eight moments, each a list of action lines: `crowned`, `dethroned`, `claim-blocked`, `knocked-off`, `fell`,
`paid`, `joined` and `left`. A moment written `[]` is silent; a moment left out of the file runs its shipped lines.
Every moment, who it reaches, the values it reads and the line grammar are on [Moments](../arenas/moments.md).

Related: [Configuration](index.md), [Arena files](arena-files.md), [Moments](../arenas/moments.md)
