---
title: Installation
order: 202
description: Requirements, the first start, the files that appear, updating and checking the install.
icon: download
---

This manual describes uxmJoinItems after 0.0.2: German, Spanish, French, Portuguese and Russian arrive with the next
release, and 0.0.2 itself reads English and Turkish.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia; every item is placed on the thread that owns the player |

## Optional plugins

| Plugin | Adds |
|---|---|
| WorldGuard | `regions` on an item |
| PlaceholderAPI | `skip-when` guards, and placeholders in action lines |
| Vault | `[give-money]` and `[take-money]` in action lines |
| mcMMO | `%mcmmo_level_<skill>%` in action lines, read without PlaceholderAPI |
| uxmLang | One language choice for every UXM plugin |

## Installing

1. Drop `uxmJoinItems-<version>.jar` into `plugins/` and remove any older jar.
2. Start the server. The plugin writes its folder on the first boot.
3. Rejoin: the compass is in slot 0, the ender chest in 4, the clock in 8.

## The files

```
plugins/uxmJoinItems/
├── config.conf            language, the default cooldown, the proxy channel
├── commands.conf          the name, aliases and switch of the command and each subcommand
├── items/                 one file per item: selector.conf, cosmetics.conf, lobby.conf
├── events/                one file per event: join.conf, first-join.conf, respawn.conf, world-change.conf
├── messages/              messages_<language>.conf: en, tr, de, es, fr, pt, ru
├── languages.conf         the language each player chose, when uxmLang is not installed
└── backups/               the last five copies of each file a release rewrote
plugins/uxmTheme/
└── theme.conf             the colours and glyphs every UXM plugin reads
```

The shipped items open other plugins: `selector` runs `menu open serverselector`, `cosmetics` runs `cosmetics`, and
`lobby` runs `lobby`. Point them at the commands your server has.

## Updating

Stop the server, replace the jar and start it. A release that adds a key writes it into your `config.conf` with its
comment and never changes a value you wrote; your file as it was is kept in `backups/`.

## Checking the install

`/uxmjoinitems doctor` (permission `uxmjoinitems.admin.doctor`):

| Check | ok | fail |
|---|---|---|
| `content` | How many items were read | None: a new player is handed nothing |

## Metrics

The plugin sends anonymous usage counts to bStats (plugin id 34246). Turn it off for every plugin in
`plugins/bStats/config.yml`.

Related: [Concepts](concepts.md), [Configuration](../config/index.md)
