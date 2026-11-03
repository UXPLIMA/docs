---
title: Installation
order: 202
description: Requirements, the first start, and the files that appear.
icon: download
---

uxmJumpPads is one jar with uxmLib inside it. Put it in `plugins/` and start: the two shipped pads work at once. This
manual describes uxmJumpPads after 0.0.1: German, Spanish, French, Portuguese and Russian arrive with the next
release, and 0.0.1 itself reads English and Turkish.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia; any fork of Paper, such as Purpur or Leaf |
| Database | none |

## Optional plugins

uxmJumpPads needs no other plugin.

| Plugin | Adds |
|---|---|
| PlaceholderAPI | Other plugins' placeholders, such as `%mcmmo_level_mining%` with the mcMMO expansion, inside an effect line's `{if=...}` condition |
| Vault with an economy | `[take-money]` and `[give-money]` in an effect line |
| uxmLang | One language choice for every UXM plugin |

## Installing

1. Put `uxmJumpPads-<version>.jar` in `plugins/` and remove any older uxmJumpPads jar.
2. Start the server. The plugin writes its files into `plugins/uxmJumpPads/`.
3. Put a light weighted pressure plate on a redstone block, walk onto it, and you are thrown.

## The files

```
plugins/uxmJumpPads/
├── config.conf       language, the pad types, the cooldown, the fall grace
├── pads.conf         the pads set on single blocks, written by /uxmjumppads set and remove
├── commands.conf     the name, aliases and switch of every command and subcommand
├── languages.conf    the language each player chose, when uxmLang is not installed
├── messages/         messages_<language>.conf, one per language: en, tr, de, es, fr, pt, ru
└── backups/          the last five copies of each file a release rewrote
plugins/uxmTheme/
└── theme.conf        the colours and glyphs every UXM plugin reads, written by the first one installed
```

`pads.conf` appears with the first `set`.

## Updating

Stop the server, replace the jar and start it. A release that adds a setting writes it into your `config.conf` with
the comment that explains it, and never changes a value you set; the file as it was goes to `backups/`. The `pads`
map is yours: a pad type you deleted is never put back.

## Checking the install

`/uxmjumppads doctor` (permission `uxmjumppads.admin.doctor`) answers one line per check:

| Check | ok | warn or fail |
|---|---|---|
| `pads` | How many pad types were read | warn: the types that were left out and why; fail: no type could be read, so nothing throws |
| `placed pads` | How many pads are set on blocks | warn: set pads that name a type `config.conf` no longer has |

`/uxmjumppads version` prints the version the server runs.
