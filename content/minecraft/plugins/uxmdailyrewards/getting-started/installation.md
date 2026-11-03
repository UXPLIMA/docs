---
title: Installation
order: 202
description: Requirements, the optional plugins and what each adds, the first start, and the files that appear.
icon: download
---

uxmDailyRewards is one jar with uxmLib inside it. Put it in `plugins/` and start: the 28 shipped days work at once.
This manual describes uxmDailyRewards after 0.1.0: German, Spanish, French, Portuguese and Russian arrive with the next
release, and 0.1.0 itself reads English and Turkish.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia; any fork of Paper, such as Purpur or Leaf |
| Database | none for one server: SQLite is built in. MySQL, MariaDB or PostgreSQL to share streaks across a network |

The database drivers are downloaded on the first start, so the first start needs internet access.

## Optional plugins

uxmDailyRewards needs no other plugin. The shipped days pay in items and experience, which every server has.

| Plugin | Adds |
|---|---|
| LuckPerms | Ranks read from the player's groups. Without it, ranks come from permission nodes |
| Vault with an economy | `[give-money] vault <amount>` in a day |
| VaultUnlocked | `[give-money] vault-unlocked <amount>` |
| PlayerPoints | `[give-money] playerpoints <amount>` |
| EcoBits | `[give-money] ecobits:<currency> <amount>` |
| Treasury | `[give-money] treasury <amount>` |
| PlaceholderAPI | The five [placeholders](../placeholders/reference.md), and other plugins' placeholders in a line's `{if=...}` condition |
| mcMMO | `%mcmmo_level_<skill>%` in a line's condition, read from mcMMO itself, without PlaceholderAPI |
| Geyser or Floodgate | The calendar as a Bedrock form for Bedrock players |
| uxmLang | One language choice for every UXM plugin |

## Installing

1. Put `uxmDailyRewards-<version>.jar` in `plugins/` and remove any older uxmDailyRewards jar.
2. Start the server. The plugin writes its files into `plugins/uxmDailyRewards/`.
3. Join. Five seconds later a title says day 1 is waiting. Run `/dailyrewards` and click the ready day.

## The files

```
plugins/uxmDailyRewards/
├── config.conf       language, storage, the claim window, the calendar length, ranks, reminder, celebration
├── days/             1.conf to 28.conf: the icon and the reward of each day
├── menus/            calendar.conf, the window
├── commands.conf     the name, aliases and switch of every command and subcommand
├── languages.conf    the language each player chose, when uxmLang is not installed
├── messages/         messages_<language>.conf, one per language: en, tr, de, es, fr, pt, ru
├── data/             dailyrewards.db, the SQLite file, while storage.type is sqlite
└── backups/          the last five copies of each file a release rewrote
plugins/uxmTheme/
└── theme.conf        the colours and glyphs every UXM plugin reads, written by the first one installed
```

## Updating

Stop the server, replace the jar and start it. A release that adds a setting writes it into your `config.conf` with
the comment that explains it, and never changes a value you set; the file as it was goes to `backups/`. The tables
are moved forward on their own. See [What an update does](../config/index.md#what-an-update-does).

## Checking the install

`/uxmdailyrewards doctor` (permission `uxmdailyrewards.admin.doctor`) answers one line per check:

| Check | ok | warn or fail |
|---|---|---|
| `storage` | The backend, the file or `host:port/database`, the prefix and the schema version | fail: the database did not answer; no streak can be read or written |
| `windows` | How many window files were read | fail: none was read, so the calendar cannot open |
| `placeholders` | `published` | warn: PlaceholderAPI is not on the server; everything else works |

At start the console also names every day without a file, every reward line it could not read, and every line that
names an economy the server does not run.

## Metrics

The plugin sends anonymous usage counts to bStats (plugin id 34240). Turn it off for every plugin in `plugins/bStats/config.yml`.
