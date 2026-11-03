---
title: Installation
order: 202
description: Requirements, the optional plugins and what each adds, the first start, and the files that appear.
icon: download
---

uxmLevels is one jar with uxmLib inside it. Put it in `plugins/` and start: the three example tracks pay at once.
This manual describes uxmLevels after 0.1.0: German, Spanish, French, Portuguese and Russian arrive with the next
release, and 0.1.0 itself reads English and Turkish.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia; any fork of Paper, such as Purpur or Leaf |
| Database | none for one server: SQLite is built in. MySQL, MariaDB or PostgreSQL to share levels across a network |

The database drivers are downloaded on the first start, so the first start needs internet access.

## Optional plugins

uxmLevels needs no other plugin.

| Plugin | Adds |
|---|---|
| PlaceholderAPI | The [placeholders](../placeholders/reference.md), and other plugins' placeholders in a reward line's `{if=...}` condition |
| Vault, VaultUnlocked, PlayerPoints, EcoBits or Treasury | A prestige that costs money in that economy, and `[give-money]` in a reward |
| LuckPerms | Nothing of its own: permission multipliers and track nodes are read through the server, so LuckPerms works as any permission plugin |
| mcMMO | `%mcmmo_level_<skill>%` in a reward line's condition, read from mcMMO itself |
| Geyser or Floodgate | The windows as Bedrock forms for Bedrock players |
| uxmLang | One language choice for every UXM plugin |

## Installing

1. Put `uxmLevels-<version>.jar` in `plugins/` and remove any older uxmLevels jar.
2. Start the server. The plugin writes its files into `plugins/uxmLevels/`.
3. Join and break a block of stone. The boss bar shows your mining level; the experience bar shows your global level.
4. Run `/levels` to see every track.

## The files

```
plugins/uxmLevels/
├── config.conf       language, storage, economy, experience, anti-abuse, leaderboard, hud, prestige, window sounds
├── tracks/           global.conf, mining.conf, combat.conf: one file per track
├── events/           general.conf, mining.conf, combat.conf: what pays into which track
├── menus/            main.conf, rewards.conf, prestige.conf, top.conf: the windows
├── commands.conf     the name, aliases and switch of every command and subcommand
├── languages.conf    the language each player chose, when uxmLang is not installed
├── messages/         messages_<language>.conf, one per language: en, tr, de, es, fr, pt, ru
├── data/             levels.db, the SQLite file, while storage.type is sqlite
└── backups/          the last five copies of each file a release rewrote
plugins/uxmTheme/
└── theme.conf        the colours and glyphs every UXM plugin reads, written by the first one installed
```

The example tracks and events are written only when `tracks/` or `events/` does not exist yet. Once the folder is
there it is yours: a track or event file you delete stays deleted. A window file you delete is written back at the
next start.

## Updating

Stop the server, replace the jar and start it. A release that adds a setting writes it into your `config.conf` with
the comment that explains it, and never changes a value you set; the file as it was goes to `backups/`. The tables
are moved forward on their own. See [What an update does](../config/index.md#what-an-update-does).

## Checking the install

`/uxmlevels doctor` (permission `uxmlevels.admin.doctor`) answers one line per check:

| Check | ok | warn or fail |
|---|---|---|
| `storage` | The backend, the file or `host:port/database`, the prefix and the schema version | fail: the database did not answer; no level or experience is remembered |
| `windows` | How many window files were read | fail: none was read, so the menu cannot open |
| `placeholders` | `published` | warn: PlaceholderAPI is not on the server; everything else works |

At start the console also names every event whose track no file declares, every track that lost a heads-up surface
to another, and every economy the config names that the server does not run.

## Metrics

The plugin sends anonymous usage counts to bStats (plugin id 34249). Turn it off for every plugin in
`plugins/bStats/config.yml`.
