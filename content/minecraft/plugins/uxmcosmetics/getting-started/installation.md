---
title: Installation
order: 102
description: Requirements, the optional plugins and what each adds, the first start, and the files that appear.
icon: download
---

uxmCosmetics is one jar with uxmLib inside it. Put it in `plugins/` and start: the wardrobe opens, the shipped prices
are in experience points, so buying works with nothing else installed. This manual describes uxmCosmetics after
0.1.0: German, Spanish, French, Portuguese and Russian arrive with the next release, and 0.1.0 itself reads English
and Turkish.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia; any fork of Paper, such as Purpur or Leaf |
| Database | none for one server: SQLite is built in. MySQL, MariaDB or PostgreSQL to share wardrobes across a network |

The database drivers are downloaded on the first start, so the first start needs internet access.

## Optional plugins

| Plugin | Adds |
|---|---|
| Vault, VaultUnlocked, PlayerPoints, EcoBits or Treasury | Prices in that economy. See [Economies](../integrations/economies.md) |
| PlaceholderAPI | The [placeholders](../placeholders/reference.md) |
| LuckPerms or any permission plugin | Granting pieces and boxes by node |
| Geyser or Floodgate | The windows as Bedrock forms for Bedrock players |
| uxmLang | One language choice for every UXM plugin |

## Installing

1. Put `uxmCosmetics-<version>.jar` in `plugins/` and remove any older uxmCosmetics jar.
2. Start the server. The plugin writes its files into `plugins/uxmCosmetics/`.
3. Join. A nether star sits in hotbar slot 8; right click it, or run `/cosmetics`.
4. Give yourself some experience and buy a trail: `/xp add <name> 1000`.

## The files

```
plugins/uxmCosmetics/
├── config.conf       language, storage, permissions, animation, economy, opener, gadget, cleanup, menus, stations, effects
├── categories/       trail.conf, halo.conf, cloak.conf, ... : one file per family, the pieces in it
├── boxes.conf        the mystery boxes and their tables
├── stations.conf     the box stations in the world, written by /uxmcosmetics station
├── commands.conf     the name, aliases and switch of every command and subcommand
├── menus/            wardrobe.conf, category.conf, boxes.conf, roulette.conf: the windows
├── messages/         messages_<language>.conf, one per language: en, tr, de, es, fr, pt, ru
├── languages.conf    the language each player chose, when uxmLang is not installed
├── data/             cosmetics.db, the SQLite file, while storage.type is sqlite
└── backups/          the last five copies of each file a release rewrote
plugins/uxmTheme/
└── theme.conf        the colours and glyphs every UXM plugin reads, written by the first one installed
```

A file you delete is written back from the jar at the next start.

## Updating

Stop the server, replace the jar and start it. A release that adds a setting writes it into your `config.conf` with
the comment that explains it, and never changes a value you set; the file as it was goes to `backups/`. The tables
are moved forward on their own.

## Checking the install

`/uxmcosmetics doctor` (permission `uxmcosmetics.admin.doctor`) answers one line per check:

| Check | ok | fail or warn |
|---|---|---|
| `storage` | The backend, the file or `host:port/database`, the prefix and the schema version | fail: the database did not answer; nothing a player owns or wears is remembered |
| `windows` | How many window files were read | fail: none was read, so the menu cannot open |
| `content` | How many cosmetics were read | fail: none was read, so the menu is empty |
| `placeholders` | `published` | warn: PlaceholderAPI is not on the server; everything else works |

At start the console also names an economy `economy.currency` asks for that the server does not run, and every
cosmetic or box that did not parse.

## Metrics

The plugin sends anonymous usage counts to bStats (plugin id 34238). Turn it off for every plugin in
`plugins/bStats/config.yml`.

Related: [Concepts](concepts.md), [Configuration](../config/index.md)
