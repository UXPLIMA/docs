---
title: Installation
order: 102
description: Requirements, the plugins it can use, the first start, and the files that appear.
icon: download
---

uxmKotl is one jar with uxmLib inside it. Put it in `plugins/`, start once, then build an arena. This manual
describes uxmKotl 0.0.1.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia; any fork of Paper, such as Purpur or Leaf |
| Internet on first start | The server downloads the MariaDB and PostgreSQL drivers from Maven Central into its library folder, once |
| Database | A SQLite file out of the box; MySQL, MariaDB or PostgreSQL to share the records of a network ([Database](../database/index.md)) |

## Optional plugins

uxmKotl starts without any of these. Each is used when it is installed; nothing has to be switched on.

| Plugin | Adds | Without it |
|---|---|---|
| PlaceholderAPI | The `%uxmkotl_...%` placeholders, for holograms, scoreboards and chat ([Placeholders](../placeholders/reference.md)), and other plugins' placeholders inside a moment's condition | The placeholders answer nothing outside the plugin; `/kotl top` and `/kotl stats` still work |
| Vault or VaultUnlocked with an economy | Rewards that pay `vault` or `vault-unlocked` money | A `vault` line pays nothing and says so once in the console |
| PlayerPoints | Rewards that pay `playerpoints` | A `playerpoints` line pays nothing |
| EcoBits | Rewards that pay an EcoBits currency | An `ecobits:<currency>` line pays nothing |
| Treasury | Rewards that pay through Treasury | A `treasury` line pays nothing |
| mcMMO | `%mcmmo_level_<skill>%` inside a moment's condition, read from mcMMO itself | The token stays as written |
| Geyser or Floodgate | Bedrock forms instead of chest menus for Bedrock players | Bedrock players get the chest menus |

Experience points and levels need no plugin. Nothing is paid by default; see [Rewards](../arenas/rewards.md).

## Installing

1. Put `uxmKotl-<version>.jar` in `plugins/` and remove any older uxmKotl jar.
2. Start the server. The plugin writes its files into `plugins/uxmKotl/` and opens its SQLite database.
3. Build a tower with a plate on top, and turn it into an arena: [Your first arena](first-arena.md) walks through it.
4. Switch the arena on with `/uxmkotl enable <name>`. Players walk in, or type `/kotl join <name>`.

<Callout type="warning" title="Items are held in memory while a player is inside">

An arena that clears the inventory on entry keeps the player's items and game mode in the server's memory until
they leave. A clean stop gives them back to everybody inside, and a player who quits gets them back when they join
again, but a crash while a player is inside, or a restart between a player's quit and their return, loses them.
Set `clear-inventory = false` on an arena in a world where the inventory does not matter.

</Callout>

## The files

```
plugins/uxmKotl/
├── config.conf          language, storage, the crown, the outline, the plate push, the stick, rewards, economy, boards, menu sounds, moments
├── arenas/
│   └── <arena>.conf     one file per arena, written by the admin commands and the editor
├── commands.conf        the name, aliases and switch of every command and subcommand
├── languages.conf       the language each player chose, when uxmLang is not installed
├── menus/
│   ├── list.conf        the arena list, for operators
│   ├── editor.conf      the arena editor, for operators
│   ├── stats.conf       a player's own record
│   └── top.conf         the leaderboard window
├── messages/            messages_<language>.conf, one per language: en, tr, de, es, fr, pt, ru
├── data/
│   └── kotl.db          the SQLite database, when SQLite is the backend
└── backups/             the last five copies of each file a release rewrote
plugins/uxmTheme/
└── theme.conf           the colours and glyphs every UXM plugin reads, written by the first one installed
```

A file you delete is written again from the jar at the next start. `arenas/` starts empty.

## Updating

Stop the server, replace the jar and start it. A release that adds a setting writes it into your files with the
comment that explains it, and never changes a value you set; the file as it was goes to `backups/`. The arena files
are yours and are never rewritten by an update. The database moves forward on its own at the start.

## Checking the install

`/uxmkotl doctor` (permission `uxmkotl.admin.doctor`) answers one line per check:

| Check | ok | warn or fail |
|---|---|---|
| `storage` | The backend, where the rows are, the table prefix and the schema version | fail: the database did not answer, so no king, no reign and no reward is recorded |
| `windows` | How many menu files were read | fail: no menu file was read, so no menu opens |
| `placeholders` | `published` | warn: PlaceholderAPI is not installed, so the placeholders publish nothing |

An arena file the plugin could not read is skipped with the reason in the console at the start and on reload.
`/uxmkotl version` prints the version the server runs.
