---
title: Installation
order: 102
description: Requirements, the plugins it can use, the first start, and the files that appear.
icon: download
---

uxmEasterEggs is one jar with uxmLib inside it. Put it in `plugins/`, start once, then place eggs. This manual
describes uxmEasterEggs after 0.1.0: German, Spanish, French, Portuguese and Russian arrive with the next release,
and 0.1.0 itself reads English and Turkish.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia; any fork of Paper, such as Purpur or Leaf |
| Internet on first start | The server downloads the MariaDB and PostgreSQL drivers from Maven Central into its library folder, once |
| Database | A SQLite file out of the box; MySQL, MariaDB or PostgreSQL to share the finds of a network ([Database](../database/index.md)) |

## Optional plugins

uxmEasterEggs starts without any of these. Each is used when it is installed; nothing has to be switched on.

| Plugin | Adds | Without it |
|---|---|---|
| PlaceholderAPI | The `%uxmeastereggs_...%` placeholders ([Placeholders](../placeholders/reference.md)), and other plugins' placeholders in a condition | The placeholders answer nothing; the menus and `/eggs top` still work |
| Vault or VaultUnlocked with an economy | Rewards that pay `vault` or `vault-unlocked` money | A `vault` line pays nothing and says so once in the console |
| PlayerPoints | Rewards that pay `playerpoints` | A `playerpoints` line pays nothing |
| EcoBits | Rewards that pay an EcoBits currency | An `ecobits:<currency>` line pays nothing |
| Treasury | Rewards that pay through Treasury | A `treasury` line pays nothing |
| mcMMO | `%mcmmo_level_<skill>%` inside a condition, read from mcMMO itself | The token stays as written |
| Geyser or Floodgate | Bedrock forms instead of chest menus for Bedrock players | Bedrock players get the chest menus |

The shipped rewards pay experience, which needs no plugin. See [Rewards](../hunting/rewards.md).

## Installing

1. Put `uxmEasterEggs-<version>.jar` in `plugins/` and remove any older uxmEasterEggs jar.
2. Start the server. The plugin writes its files into `plugins/uxmEasterEggs/` and opens its SQLite database.
3. Place the blocks that will be eggs, then register each one: [Your first hunt](first-hunt.md) walks through it.
4. Players open their collection with `/eggs`.

<Callout type="warning" title="An egg block cannot be broken, even by you">

A registered block is protected from everybody, operators included. To move or remove an egg block, first take the
egg off the map with `/uxmeastereggs delete` while you look at it, or move it with `set <egg> position`.

</Callout>

## The files

```
plugins/uxmEasterEggs/
├── config.conf          language, storage, finds, radar, particles, economy, milestones, completion, boards, menus, moments
├── types/
│   ├── common.conf      one file per egg type
│   └── golden.conf
├── commands.conf        the name, aliases and switch of every command and subcommand
├── languages.conf       the language each player chose, when uxmLang is not installed
├── menus/
│   ├── collection.conf  a player's collection
│   ├── leaderboard.conf the top finders
│   ├── admin.conf       the placed eggs, for operators
│   └── picker.conf      the type picker, for operators
├── messages/            messages_<language>.conf, one per language: en, tr, de, es, fr, pt, ru
├── data/
│   └── eggs.db          the SQLite database, when SQLite is the backend
└── backups/             the last five copies of each file a release rewrote
plugins/uxmTheme/
└── theme.conf           the colours and glyphs every UXM plugin reads, written by the first one installed
```

The placed eggs and the finds are in the database, not in a file.

## Updating

Stop the server, replace the jar and start it. A release that adds a setting writes it into your `config.conf` with
the comment that explains it, and never changes a value you set; the file as it was goes to `backups/`. The
milestones you wrote stay as you left them, and the type files are yours. The database moves forward on its own at
the start.

## Checking the install

`/uxmeastereggs doctor` (permission `uxmeastereggs.admin.doctor`) answers one line per check:

| Check | ok | warn or fail |
|---|---|---|
| `storage` | The backend, where the rows are, the table prefix and the schema version | fail: the database did not answer, so no find is remembered |
| `windows` | How many menu files were read | fail: no menu file was read, so no menu opens |
| `placeholders` | `published` | warn: PlaceholderAPI is not installed, so the placeholders publish nothing |

`/uxmeastereggs version` prints the version the server runs.
