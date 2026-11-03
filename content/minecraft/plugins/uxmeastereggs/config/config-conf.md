---
title: config.conf
order: 601
description: Every key of the main file, with its shipped value and what it does.
icon: file-cog
---

The main settings file. Every key is shown with the value the plugin ships. `/uxmeastereggs reload` applies a change
unless the key says otherwise. The egg types are not here but in [types/](type-files.md), and the wording of every
line in the [messages files](messages.md).

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
| `file` | `"eggs.db"` | The SQLite file, inside `data/`. Read only for `sqlite` |
| `host` | `"localhost"` | The database server, for the other three types |
| `port` | `0` | Its port; `0` is the type's own, 3306 for MySQL and MariaDB, 5432 for PostgreSQL |
| `database` | `"minecraft"` | The database name |
| `username` | `"minecraft"` | The user |
| `password` | `""` | The password |
| `table-prefix` | `"uxmeggs_"` | Written in front of every table, so two plugins, or two copies of one, can share a database. Letters, digits and underscores |
| `pool-size` | `4` | How many connections the pool may open; SQLite always uses one |
| `sync-seconds` | `2` | How often this server reads what other servers on the same database changed for a player who is here: the eggs they have found; `0` turns it off. SQLite reads nothing |
| `local-only` | `false` | This one server keeps its rows in the SQLite file whatever `type` says, for a server of a network that has no business in the shared database |
| `copy-from-file` | `false` | Copies the rows of the SQLite file into the empty tables of the new database once, at the start |

The storage is opened once: a change needs a restart. See [Database](../database/index.md) for each backend and
for moving between them.

## find

| Key | Default | What it does |
|---|---|---|
| `announce-progress` | `true` | Runs the `progress` moment after each first find: *You have found 4 of 60 eggs.* Turn it off if your type effects already say it |

## radar

| Key | Default | What it does |
|---|---|---|
| `default-on` | `false` | Whether a player who has not used `/eggs radar` has the radar on. A change needs a restart |
| `radius` | `48` | How far the radar looks, in blocks |
| `interval-ms` | `3000` | How often the radar task wakes, in milliseconds |
| `action-bar` | `true` | The hint over the hotbar; `false` sends it in chat |

## particles

| Key | Default | What it does |
|---|---|---|
| `interval-ms` | `500` | How often the idle particle task wakes. Above 0 |
| `radius` | `24` | How far from a player an unfound egg is drawn, in blocks. Above 0 |
| `max-eggs-per-player` | `8` | The most eggs drawn for one player at once, at least 1 |

See [Radar and particles](../hunting/radar-and-particles.md).

## economy

| Key | Default | What it does |
|---|---|---|
| `treasury-wait-ms` | `250` | How long one Treasury call may take before the payment is given up on. Above 0 |

## milestones

| Key | Default | What it does |
|---|---|---|
| `first` | `at = 5`, `money = ["exp 250"]`, `broadcast = false` | A reward at 5 eggs |
| `half` | `at = 25`, `money = ["exp 1000"]`, `broadcast = true` | A reward at 25 eggs, told to the server |

Each block is one milestone with `at`, `money`, `commands` and `broadcast`; the block name is yours. The milestones are
yours: an update never puts back one you removed. See [Rewards](../hunting/rewards.md#milestones).

## completion

| Key | Default | What it does |
|---|---|---|
| `money` | `["exp 5000"]` | Paid for finding every egg on the map |
| `commands` | `[]` | Console commands; `%player%` is the finder's name |
| `broadcast` | `true` | Also runs `completion-broadcast`, told to the server |

## leaderboard

| Key | Default | What it does |
|---|---|---|
| `size` | `10` | How many places each board keeps; a place past this names nobody |
| `refresh-seconds` | `15` | How often the boards are read from the database. A find shows at the next read. Needs a restart |
| `empty-head` | `"entity/player/wide/steve"` | What `%uxmeastereggs_top_<board>_<period>_<n>_head%` gives for a place nobody holds: a texture path, a player's name or id |
| `periods` | `[ alltime, monthly, weekly, daily ]` | The periods each board is kept for; an empty list keeps all four |
| `time-zone` | `""` | The zone a day starts in, such as `"Europe/Istanbul"`; empty is the server's. Servers sharing a database should write the same |
| `week-starts` | `monday` | The day a week starts on |
| `keep-windows` | `12` | How many ended windows of a period are kept, so last week's winner can still be read; older ones are deleted |

The board is `found`, the first finds of the period; a repeat find moves no board. See [Placeholders](../placeholders/reference.md#boards).

## menu

| Key | Default | What it does |
|---|---|---|
| `sounds.click` | `block.note_block.pling`, volume `0.6`, pitch `1.5` | A button that does something, unless its own actions play a sound |
| `sounds.page` | `item.book.page_turn`, `0.7`, `1.0` | A page arrow |
| `sounds.denied` | `block.note_block.bass`, `0.6`, `0.9` | A button the player may not use |
| `leaderboard.size` | `10` | How many players the [leaderboard window](../menus/leaderboard.md) reads, at least 1 |
| `collection.hide-missing` | `false` | Draws a type the player has not found one of as a grey dye instead of its head |

## effects

The seven moments of a hunt, each a list of action lines: `progress`, `milestone`, `milestone-broadcast`,
`completion`, `completion-broadcast`, `radar-on` and `radar-off`. What one egg does when it is found is the `effects`
list of its type. A moment written `[]` is silent; a moment left out of the file runs its shipped lines. See
[Moments](../hunting/moments.md).

Related: [Configuration](index.md), [Type files](type-files.md), [Moments](../hunting/moments.md)
