---
title: Installation
order: 202
description: Requirements, the optional plugins, the first start, the files that appear, updating and checking the install.
icon: download
---

This manual describes uxmAuction after 0.1.0: German, Spanish, French, Portuguese and Russian arrive with the next
release, and 0.1.0 itself reads English and Turkish.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia; money work runs off the main threads, items and experience on the thread that owns the player |
| Database | None for one server: SQLite is built in. MySQL, MariaDB or PostgreSQL for a network |

The database drivers are downloaded on the first start, so the first start needs internet access.

## Optional plugins

| Plugin | Adds |
|---|---|
| Vault, VaultUnlocked | The `coins` currency, or `money` |
| PlayerPoints, EcoBits, Treasury | More currencies |
| PlaceholderAPI | The [placeholders](../placeholders/reference.md), and the `placeholder` currency type |
| Geyser or Floodgate | Windows as Bedrock forms |
| mcMMO | `%mcmmo_level_<skill>%` in effect lines |
| uxmLang | One language choice for every UXM plugin |

## Installing

1. Drop `uxmAuction-<version>.jar` into `plugins/`.
2. Start the server once. The plugin writes `plugins/uxmAuction/`.
3. Open `currencies.conf`: `coins` (Vault) and `experience` ship on. Turn on the ones your server runs.
4. `/uxmauction reload`.

A currency whose plugin is missing stays listed and says it is not ready; listings in it are refused until the plugin is
installed.

## The files

```
plugins/uxmAuction/
├── config.conf        language, house rules, history, events, webhook, placeholders, asking, item lines, admin, storage, network, fees, effects, menu sounds
├── currencies.conf    the currencies the house prices in and how amounts are written
├── categories.conf    the tabs of the house and which items fall into each
├── commands.conf      the name, aliases and switch of every command and subcommand
├── menus/             house, category, listing, sell, search, mailbox, mine, bids, orders, order-create, watch, history, profile, admin/log
├── messages/          messages_<language>.conf: en, tr, de, es, fr, pt, ru
├── languages.conf     the language each player chose, when uxmLang is not installed
├── data/              auction.db, the SQLite file, while storage.type is sqlite
└── backups/           the last five copies of each file a release rewrote
plugins/uxmTheme/
└── theme.conf         the colours and glyphs every UXM plugin reads
```

## Updating

Stop the server, replace the jar and start it. A release that adds a key writes it into your `config.conf` with its
comment and never changes a value you wrote; your file as it was is kept in `backups/`. On a network, update every server
together.

## Checking the install

`/uxmauction doctor` (permission `uxmauction.admin.doctor`):

| Check | ok | fail or warn |
|---|---|---|
| `storage` | The backend, its place, the prefix and the schema version | fail: the database did not answer |
| `windows` | How many window files were read | fail: none, so the house cannot open |
| `placeholders` | `published` | warn: PlaceholderAPI is not on the server; everything else works |

Whether a currency is ready is not one of the checks: a listing in a currency that is not ready says so when it is
refused, and the console names it at start.

## Metrics

The plugin sends anonymous usage counts to bStats (plugin id 34235). Turn it off for every plugin in
`plugins/bStats/config.yml`.

Related: [Concepts](concepts.md), [Configuration](../config/index.md)
