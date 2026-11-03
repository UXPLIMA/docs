---
title: Installation
order: 102
description: Requirements, the optional plugins and what each adds, the first start, and the files that appear.
icon: download
---

uxmPlayerHider is one jar with uxmLib inside it. Put it in `plugins/` and start: every player who joins gets the tool.
This manual describes uxmPlayerHider after 0.1.0: German, Spanish, French, Portuguese and Russian arrive with the next
release, and 0.1.0 itself reads English and Turkish.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia; any fork of Paper, such as Purpur or Leaf |
| Database | none for one server: SQLite is built in. MySQL, MariaDB or PostgreSQL to share the choice across a network |

The database drivers are downloaded on the first start, so the first start needs internet access.

## Optional plugins

uxmPlayerHider needs no other plugin.

| Plugin | Adds |
|---|---|
| PlaceholderAPI | The three [placeholders](../placeholders/reference.md), and other plugins' placeholders inside an effect line's `{if=...}` condition |
| Vault with an economy | `[take-money]` and `[give-money]` in an effect line |
| mcMMO | `%mcmmo_level_<skill>%` in an effect line's condition, read from mcMMO itself, without PlaceholderAPI |
| uxmLang | One language choice for every UXM plugin |

## Installing

1. Put `uxmPlayerHider-<version>.jar` in `plugins/` and remove any older uxmPlayerHider jar.
2. Start the server. The plugin writes its files into `plugins/uxmPlayerHider/`.
3. Join. The tool is a lime dye in hotbar slot 8 (slot `7`, counted from 0). Right click it.
4. Name your lobby worlds in `worlds` in `config.conf` so the tool does nothing elsewhere, and run
   `/uxmplayerhider reload`.

Installing on a running server, or a plugin reload, brings the players already online up to date at once: they get
the tool and their saved state without logging out.

## The files

```
plugins/uxmPlayerHider/
├── config.conf       language, storage, worlds, cooldown, the tool, the effects of each state
├── commands.conf     the name, aliases and switch of every command and subcommand
├── languages.conf    the language each player chose, when uxmLang is not installed
├── messages/         messages_<language>.conf, one per language: en, tr, de, es, fr, pt, ru
├── data/             playerhider.db, the SQLite file, while storage.type is sqlite
└── backups/          the last five copies of each file a release rewrote
plugins/uxmTheme/
└── theme.conf        the colours and glyphs every UXM plugin reads, written by the first one installed
```

## Updating

Stop the server, replace the jar and start it. A release that adds a setting writes it into your `config.conf` with
the comment that explains it, and never changes a value you set; the file as it was goes to `backups/`. The tables
are moved forward on their own. See [What an update does](../config/index.md#what-an-update-does).

## Checking the install

`/uxmplayerhider doctor` (permission `uxmplayerhider.admin.doctor`) answers one line per check:

| Check | ok | warn or fail |
|---|---|---|
| `storage` | The backend and where the rows are | fail: the database did not answer, so no choice is remembered between sessions |
| `placeholders` | `published` | warn: PlaceholderAPI is not on the server; everything else works |

`/uxmplayerhider version` prints the version the server runs.

## Metrics

The plugin sends anonymous usage counts to bStats (plugin id 34254). Turn it off for every plugin in
`plugins/bStats/config.yml`.
