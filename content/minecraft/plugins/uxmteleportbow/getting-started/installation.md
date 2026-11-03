---
title: Installation
order: 102
description: Requirements, the optional plugins and what each adds, the first start, and the files that appear.
icon: download
---

uxmTeleportBow is one jar with uxmLib inside it. Put it in `plugins/` and start: every player who joins gets the bow.
This manual describes uxmTeleportBow after 0.0.2: German, Spanish, French, Portuguese and Russian arrive with the next
release, and 0.0.2 itself reads English and Turkish.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia; any fork of Paper, such as Purpur or Leaf |
| Database | none |

## Optional plugins

uxmTeleportBow needs no other plugin.

| Plugin | Adds |
|---|---|
| WorldGuard | Region names for the [region filter](../bow/filters.md#regions) |
| Towny | Town names for the same filter: a town's claim counts as a region named after the town |
| PlaceholderAPI | The four [placeholders](../placeholders/reference.md), and other plugins' placeholders inside an effect line's `{if=...}` condition |
| Vault with an economy | `[take-money]` and `[give-money]` in an effect line |
| mcMMO | `%mcmmo_level_<skill>%` in an effect line's condition, read from mcMMO itself, without PlaceholderAPI |
| uxmLang | One language choice for every UXM plugin |

## Installing

1. Put `uxmTeleportBow-<version>.jar` in `plugins/` and remove any older uxmTeleportBow jar.
2. Start the server. The plugin writes its files into `plugins/uxmTeleportBow/`.
3. Join. The bow is in hotbar slot 2 (slot `1`, counted from 0) and one arrow is in the top left slot of the
   inventory.
4. Before you open the server, close the worlds where the bow must not work: see [Filters](../bow/filters.md).

## The files

```
plugins/uxmTeleportBow/
├── config.conf       language, the bow and arrow, the teleport rules, the trail, the effects, the filters
├── commands.conf     the name, aliases and switch of the command and its subcommands
├── languages.conf    the language each player chose, when uxmLang is not installed
├── messages/         messages_<language>.conf, one per language: en, tr, de, es, fr, pt, ru
└── backups/          the last five copies of each file a release rewrote
plugins/uxmTheme/
└── theme.conf        the colours and glyphs every UXM plugin reads, written by the first one installed
```

`languages.conf` appears with the first player who chooses a language. The plugin keeps no database: a cooldown, a
fall protection and who holds the bow are kept in memory and forgotten when the player leaves.

## Updating

Stop the server, replace the jar and start it. A release that adds a setting writes it into your `config.conf` with
the comment that explains it, and never changes a value you set; the file as it was goes to `backups/`. See
[What an update does](../config/index.md#what-an-update-does).

## Checking the install

`/uxmteleportbow doctor` (permission `uxmteleportbow.admin.doctor`) answers one line per check:

| Check | ok | warn |
|---|---|---|
| `placeholders` | `published`: PlaceholderAPI is there and the four placeholders answer | PlaceholderAPI is not on the server, so the placeholders publish nothing; the rest of the plugin works |

`/uxmteleportbow version` prints the version the server runs. With `filters.regions.enabled = true` and neither
WorldGuard nor Towny installed, the console says once at start that no region is checked.

## Metrics

The plugin sends anonymous usage counts to bStats (plugin id 34260). Turn it off for every plugin in
`plugins/bStats/config.yml`.
