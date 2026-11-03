---
title: Installation
order: 102
description: Requirements, the plugins it can use, the first start, and the files that appear.
icon: download
---

uxmHubPvP is one jar with uxmLib inside it. Put it in `plugins/` and start: the lobby can fight at once. This manual
describes uxmHubPvP after 0.1.0: German, Spanish, French, Portuguese and Russian arrive with the next release, and
0.1.0 itself reads English and Turkish.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia; any fork of Paper, such as Purpur or Leaf |
| Internet on first start | The server downloads the MariaDB and PostgreSQL drivers from Maven Central into its library folder, once |
| Database | A SQLite file out of the box; MySQL, MariaDB or PostgreSQL to share the records of a network ([Database](../database/index.md)) |

## Optional plugins

uxmHubPvP starts without any of these. Each is used when it is installed; nothing has to be switched on.

| Plugin | Adds | Without it |
|---|---|---|
| PlaceholderAPI | The `%uxmhubpvp_...%` placeholders ([Placeholders](../placeholders/reference.md)), other plugins' placeholders in a moment's condition, and the guards that step aside for another plugin's game ([Other games](../integrations/other-games.md)) | The placeholders answer nothing and the guards never apply; `/pvp stats` and `/pvp top` still work |
| Vault or VaultUnlocked with an economy | Kills that pay `vault` or `vault-unlocked` money | A `vault` line pays nothing and says so once in the console |
| PlayerPoints | Kills that pay `playerpoints` | A `playerpoints` line pays nothing |
| EcoBits | Kills that pay an EcoBits currency | An `ecobits:<currency>` line pays nothing |
| Treasury | Kills that pay through Treasury | A `treasury` line pays nothing |
| mcMMO | `%mcmmo_level_<skill>%` inside a moment's condition, read from mcMMO itself | The token stays as written |
| uxmGlow | The colour a fighter's outline glows in | Fighters glow white |

A kill pays 10 experience points as shipped, which needs no plugin. See [Rewards](../fighting/rewards.md).

## Installing

1. Put `uxmHubPvP-<version>.jar` in `plugins/` and remove any older uxmHubPvP jar.
2. Start the server. The plugin writes its files into `plugins/uxmHubPvP/` and opens its SQLite database.
3. Every player who joins gets the toggle sword in hotbar slot 2. Clicking it starts the fight.
4. Fit it to your lobby: [Setting up the fight](first-fight.md) names the settings most lobbies change.

<Callout type="warning" title="Give the toggle item a slot of its own">

The sword goes to hotbar slot 2 when that slot is empty, and to the first free slot otherwise. A lobby whose other
plugins hand out items in slot 2 pushes it somewhere else on every join. Set `toggle-item.slot` to a slot nothing else
uses.

</Callout>

## The files

```
plugins/uxmHubPvP/
├── config.conf      language, storage, arena rules, warm-ups, combat tag, toggle item, respawn, visuals, rewards, economy, boards, moments
├── kit.conf         the kit a fighter is handed, written once and never merged
├── commands.conf    the name, aliases and switch of every command and subcommand
├── languages.conf   the language each player chose, when uxmLang is not installed
├── messages/        messages_<language>.conf, one per language: en, tr, de, es, fr, pt, ru
├── data/
│   └── hubpvp.db    the SQLite database, when SQLite is the backend
└── backups/         the last five copies of each file a release rewrote
plugins/uxmTheme/
└── theme.conf       the colours and glyphs every UXM plugin reads, written by the first one installed
```

A file you delete is written again from the jar at the next start.

## Updating

Stop the server, replace the jar and start it. A release that adds a setting writes it into your `config.conf` with
the comment that explains it, and never changes a value you set; the file as it was goes to `backups/`. `kit.conf`
is yours and is never merged: a piece you removed stays removed. An older `config.conf` that still carries a `kit`
block has it moved into `kit.conf` once. The database moves forward on its own at the start.

## Checking the install

`/uxmhubpvp doctor` (permission `uxmhubpvp.admin.doctor`) answers one line per check:

| Check | ok | warn or fail |
|---|---|---|
| `storage` | The backend, where the rows are, the table prefix and the schema version | fail: the database did not answer, so no kill, death or streak is counted |
| `placeholders` | `published` | warn: PlaceholderAPI is not installed, so the placeholders publish nothing |

`/uxmhubpvp version` prints the version the server runs.
