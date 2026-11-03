---
title: Installation
order: 102
description: Requirements, the first start, the files that appear, updating and checking the install.
icon: download
---

This manual describes uxmLang after 0.1.0: German, Spanish, French, Portuguese and Russian, each language's flag in the
window and language names in small capitals arrive with the next release, and 0.1.0 itself reads English and Turkish.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia; any fork of Paper |
| Database | None for one server: SQLite is built in. MySQL, MariaDB or PostgreSQL to share choices across a network |
| Redis | Only for instant changes across a network; the driver is in the jar |

The database drivers are downloaded on the first start, so the first start needs internet access.

## Optional plugins

| Plugin | Adds |
|---|---|
| Geyser or Floodgate | The window as a Bedrock form |
| PlaceholderAPI | Placeholders in effect lines |
| Vault | `[give-money]` and `[take-money]` in effect lines |
| mcMMO | `%mcmmo_level_<skill>%` in effect lines, read without PlaceholderAPI |

## Installing

1. Drop `uxmLang-<version>.jar` into `plugins/` and remove any older uxmLang jar.
2. Start the server. The plugin writes its folder on the first boot.
3. Run `/lang` and pick a language. Every UXM plugin now draws your lines in it.

## The files

```
plugins/uxmLang/
├── config.conf        language, storage, network, effects, window sounds
├── commands.conf      the name, aliases and switch of every command and subcommand
├── languages.conf     the name and icon of each language
├── menus/language.conf   the language window
├── messages/          messages_<language>.conf, one per language: en, tr, de, es, fr, pt, ru
├── data/              languages.db, the SQLite file, while storage.type is sqlite
└── backups/           the last five copies of each file a release rewrote
plugins/uxmTheme/
└── theme.conf         the colours and glyphs every UXM plugin reads
```

## Updating

Stop the server, replace the jar and start it. A release that adds a key writes it into your `config.conf` with its
comment and never changes a value you wrote; your file as it was is kept in `backups/`.

## Checking the install

`/uxmlang doctor` (permission `uxmlang.admin.doctor`):

| Check | ok | fail |
|---|---|---|
| `storage` | The backend, its place, the prefix and the schema version | The database did not answer: no choice is remembered between sessions |
| `content` | How many languages were read | None: every line falls back to the text in the code |
| `windows` | `1 read` | The window could not be read, so it cannot open; `/lang` answers in chat |

## Metrics

The plugin sends anonymous usage counts to bStats (plugin id 34248). Turn it off for every plugin in
`plugins/bStats/config.yml`.

Related: [Concepts](concepts.md), [Configuration](../config/index.md)
