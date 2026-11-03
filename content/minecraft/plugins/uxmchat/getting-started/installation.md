---
title: Installation
order: 202
description: Requirements, the optional plugins, the first start, the files that appear, updating and checking the install.
icon: download
---

uxmChat is one jar with uxmLib inside it, plus two optional jars: one for the Velocity proxy and one for Discord. This
manual describes uxmChat after 0.2.0: German, Spanish, French, Portuguese and Russian arrive with the next release,
and 0.2.0 itself reads English and Turkish.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia; any fork of Paper. Every piece of work runs on the thread that owns the player |
| Accounts | Online or offline mode, Java and Bedrock players through Geyser or Floodgate |
| Database | None for one server: SQLite is built in. MySQL, MariaDB or PostgreSQL for a network |

The database drivers are downloaded on the first start, so the first start needs internet access.

## The jars

| Jar | Goes in | For |
|---|---|---|
| `uxmChat-<version>.jar` | every server's `plugins/` | Everything on this page |
| `uxmChat-Velocity-<version>.jar` | the proxy's `plugins/` | A network: see [A network](../features/network.md) |
| `uxmChat-Discord-<version>.jar` | one server's `plugins/`, beside uxmChat | Discord: see [Discord](../features/discord.md) |

## Optional plugins

Every one of these is reached only when it is installed; the plugin runs fully without any of them.

| Plugin | Adds |
|---|---|
| LuckPerms | `<prefix>`, `<suffix>`, `<group>` and `<meta:key>` in formats, and group formats |
| PlaceholderAPI | `%placeholders%` in formats, announcements and events, and uxmChat's own [placeholders](../placeholders/reference.md) |
| Vault | `[money]`, the `coins` currency for tags, `[give-money]` rewards |
| PlayerPoints, Treasury | More [currencies](../config/currencies-conf.md) to sell tags in |
| AuthMe, nLogin, LibreLogin | The chat stays closed until a player logs in |
| LiteBans, LibertyBans, AdvancedBan, Essentials, uxmEssentials | Their mutes: see [Punishment plugins](../integrations/punishment-plugins.md) |
| Geyser or Floodgate | Bedrock players recognised, windows as Bedrock forms |
| uxmLang | One language choice for every UXM plugin |

## Installing

1. Drop `uxmChat-<version>.jar` into `plugins/` and remove any older uxmChat jar.
2. Start the server. The plugin writes its folder on the first boot.
3. Say something in chat. It goes to the `global` channel in the `global` format.
4. Run `/chat` to open your settings and `/channel list` to see the six shipped channels.

## The files

```
plugins/uxmChat/
├── config.conf                language, server name, storage, network, modules, chat rules and limits
├── commands.conf              the name, aliases and switch of every command and subcommand
├── channels.conf              the channels a new database starts with
├── formats.conf               how a line is drawn
├── currencies.conf            the currencies tags are sold in
├── modules/
│   ├── filters/filters.conf   filters, the ladder, automatic replies
│   ├── filters/words/         swear words, <language>.txt, one word per line
│   ├── emojis/emojis.conf     emoji
│   ├── colours/colours.conf   chat colours
│   ├── tags/tags.conf         tag categories, rarities, rules, the tags a new database starts with
│   ├── announcements/announcements.conf   the announcement bundles a new database starts with
│   ├── events/events.conf     join, quit, death, kick, advancement, unknown command and network lines
│   ├── custom-commands/custom-commands.conf   commands of your own
│   └── games/                 games.conf, words/<language>.txt, trivia/<language>.conf
├── menus/                     ten windows, one file each
├── messages/                  messages_<language>.conf: en, tr, de, es, fr, pt, ru
├── languages.conf             the language each player chose, when uxmLang is not installed
├── data/                      chat.db, the SQLite file, while storage.type is sqlite
└── backups/                   the last five copies of each file a release rewrote
plugins/uxmTheme/
└── theme.conf                 the colours and glyphs every UXM plugin reads
```

Which file is read when:

| File | Read |
|---|---|
| `config.conf` | At start and on every reload |
| `commands.conf` | At start |
| `channels.conf` | Once, into the database; then in game |
| `formats.conf`, `currencies.conf` | At start and on every reload |
| `modules/filters/filters.conf` and `words/` | At start and on every reload |
| `modules/emojis/emojis.conf`, `modules/colours/colours.conf` | At start and on every reload |
| `modules/tags/tags.conf` | Every reload; the tags themselves once, into the database |
| `modules/announcements/announcements.conf` | Once, into the database; then in game |
| `modules/events/events.conf`, `modules/games/` | At start and on every reload |
| `modules/custom-commands/custom-commands.conf` | At start |
| `messages/`, `menus/` | At start and on every reload |

## Moving from a release before the module folders

A folder from a release that kept every file at the top moves on the first start: each file goes to its module folder,
`words_<language>.txt` goes to `modules/filters/words/`, and the words and trivia that `games.conf` held are written
into their language files, with the old file kept as `games.conf.bak`. The console names everything it moved. A
SQLite file at the top of the folder moves into `data/`, its journal files with it.

## Updating

Stop the server, replace the jar and start it. A release that adds a key writes it into your `config.conf` with its
comment and never changes a value you wrote; your file as it was is kept in `backups/`. On a network, update every
server and the proxy together: what they send each other changes between versions.

## Checking the install

`/uxmchat version` names the version this server runs. `/uxmchat doctor` (permission `uxmchat.admin.doctor`) prints
a header and a closing line; it carries no checks of its own yet, so read the console after a start or a reload: a
file that could not be read is named there, with the line it stopped at.

Related: [Concepts](concepts.md), [Configuration](../config/index.md)
