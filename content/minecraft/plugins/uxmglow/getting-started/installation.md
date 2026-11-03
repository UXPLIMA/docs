---
title: Installation
order: 102
description: Requirements, the first start, the files that appear, and checking the install.
icon: download
---

uxmGlow is one jar with uxmLib inside it. This manual describes uxmGlow after 0.1.0: German, Spanish, French,
Portuguese and Russian arrive with the next release, and 0.1.0 itself reads English and Turkish.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia; any fork of Paper. Every piece of work runs on the thread that owns the player |
| Database | none for one server: SQLite is built in. MySQL, MariaDB or PostgreSQL to share choices across a network |

The database drivers are downloaded on the first start, so the first start needs internet access.

## Optional plugins

| Plugin | Adds |
|---|---|
| PlaceholderAPI | The [placeholders](../placeholders/reference.md) |
| A permission plugin, such as LuckPerms | Handing colours to ranks |
| Geyser or Floodgate | The picker as a Bedrock form |
| uxmLang | One language choice for every UXM plugin |

## Installing

1. Drop `uxmGlow-<version>.jar` into `plugins/` and remove any older uxmGlow jar.
2. Start the server. The plugin writes its folder on the first boot.
3. As an operator, run `/glow`: an operator holds every colour.

## The files

```
plugins/uxmGlow/
├── config.conf       language, storage, cycle, effects, window sounds
├── commands.conf     the name, aliases and switch of every command and subcommand
├── menus/glow.conf   the picker
├── messages/         messages_<language>.conf, one per language: en, tr, de, es, fr, pt, ru
├── languages.conf    the language each player chose, when uxmLang is not installed
├── data/             glow.db, the SQLite file, while storage.type is sqlite
└── backups/          the last five copies of each file a release rewrote
plugins/uxmTheme/
└── theme.conf        the colours and glyphs every UXM plugin reads
```

## Updating

Stop the server, replace the jar and start it. A release that adds a key writes it into your `config.conf` with its
comment and never changes a value you wrote; your file as it was is kept in `backups/` as `config-<date>.conf`.

## Checking the install

`/uxmglow doctor` (permission `uxmglow.admin.doctor`):

| Check | ok | fail or warn |
|---|---|---|
| `storage` | The backend, its place, the prefix and the schema version | fail: the database did not answer |
| `windows` | How many window files were read | fail: none was read, so the picker cannot open |
| `placeholders` | `published` | warn: PlaceholderAPI is not on the server; everything else works |

## Metrics

The plugin sends anonymous usage counts to bStats (plugin id 34243). Turn it off for every plugin in
`plugins/bStats/config.yml`.

Related: [Concepts](concepts.md), [Configuration](../config/index.md)
