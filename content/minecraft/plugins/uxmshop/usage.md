---
title: "Guide"
order: 10
description: "How uxmShop works, how to set it up and what each file holds."
---

## Install

Put `uxmShop-<version>.jar` in `plugins/` and start the server. It needs Paper with API version 26.2 or
newer and Java 25. uxmLib is inside the jar, so nothing else has to be installed.

These plugins are optional and are named as soft dependencies. The plugin runs without any of them.

| Plugin | What it adds |
|---|---|
| Vault or VaultUnlocked | The `vault` and `vaultunlocked` currencies. Needed for player shops, which must pay an owner who is away. |
| PlayerPoints, EcoBits, Treasury | The `playerpoints`, `ecobits` and `treasury` currencies. |
| PlaceholderAPI | The `%uxmshop_...%` placeholders and the `placeholder` currency type. |
| Geyser, Floodgate | Menus as forms for Bedrock players. |
| mcMMO | A line can name `%mcmmo_level_<skill>%` in a condition without PlaceholderAPI. |

The shipped catalogue is priced in experience, so a server with no economy plugin can still trade. Player
shops need an economy: see [currencies](currencies.md).

## Configure

The plugin writes these files into `plugins/uxmShop/` on first start. A file that already exists is never
overwritten.

| File | What it holds |
|---|---|
| `config.conf` | Language, storage, tax, ranks, player shop limits, the hologram, blacklist, menu sounds, log retention, the editor switch and the `effects` run after each trade. |
| `commands.conf` | The name, aliases and on/off switch of every command and subcommand. |
| `currencies.conf` | The currencies and how an amount is written. See [currencies](currencies.md). |
| `shops/<section>.conf` | One catalogue section per file. Ships `blocks.conf` and `tools.conf`. See [the catalogue](catalogue.md). |
| `menus/<window>.conf` | Where every tile of a window sits. Ships `hub`, `section`, `quantity`, `sell`, `shops` and `playershop`. |
| `messages/messages_<lang>.conf` | The text of every message, one file per language. Ships `en` and `tr`. |
| `languages.conf` | The language each player chose. Written by the plugin. |
| `data/shop.db` | The SQLite database, when `storage.type` is `sqlite`. |

The colours of every message are in `plugins/uxmTheme/theme.conf`, a file shared with the other UXPLIMA
plugins on the server.

`/uxmshop reload` reads the configuration, the catalogue, the menus, the messages and the theme again. A
reload is applied as a whole or not at all: when a file does not parse, the plugin keeps running on what it
had and the console says why. Changes to `commands.conf` and to `storage` need a restart: the command tree
is built once at enable, and the database is opened once.

When a release adds a key to `config.conf`, the next start adds it with its comment and keeps every value
already written. The file as it was is kept as `config.conf.bak`. Language files take the same care: a line
nobody changed is corrected to the new wording, and the console lists which ones.

## Storage

The default is a SQLite file, which needs no setup. Set `storage.type` to `mysql`, `mariadb` or
`postgresql` and fill in the host, database and credentials to share the rows between servers. The
MariaDB and PostgreSQL drivers are downloaded into the server's `libraries` folder at first start, so that
start needs network access.

Moving from the SQLite file to a network database: set the new type and `storage.copy-from-file = true`,
then restart once. The rows of the file are copied into the empty tables and the log says how many. Tables
that already hold rows are left alone.

`storage.local-only = true` keeps one server of a network on its own SQLite file whatever the type says.

## Languages

A player reads the language they chose with `/shop lang set <language>`, else their client's language when
`language.follow-client` is on, else `language.default`. `language.force` puts everybody on one language,
and a player can then no longer change theirs. A new language is one more file in `messages/`. An operator
sets another player's language with `/shop lang set-player`.

## Renaming commands

Every command and subcommand is named in `commands.conf`. Change the `name`, add `aliases`, or set
`enabled = false` and the command is never registered. `localized-aliases` adds words for the readers of
one language. The key on the left never changes, only the values do. The `/shopedit` command exists only
while `editor.enabled` is `true` in `config.conf`.
