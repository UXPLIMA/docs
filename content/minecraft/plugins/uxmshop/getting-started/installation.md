---
title: Installation
order: 202
description: Requirements, the database drivers, the files, updating and checking the install.
icon: download
---

This manual describes uxmShop after 0.1.0: German, Spanish, French, Portuguese and Russian arrive with the next
release, and 0.1.0 itself reads English and Turkish.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia |

uxmLib is shaded into the jar, so nothing else is required. The shipped catalogue prices in experience, so a server
with no economy plugin can still trade. Player shops need a currency that can pay an owner who is away, such as Vault.

## Optional plugins

| Plugin | Adds |
|---|---|
| Vault, VaultUnlocked | The `vault` and `vaultunlocked` currencies |
| PlayerPoints, EcoBits, Treasury | Those currencies |
| PlaceholderAPI | The [placeholders](../placeholders/reference.md), `%placeholders%` in conditions, and the `placeholder` currency |
| Floodgate, Geyser | Windows as forms for Bedrock players |
| mcMMO | `%mcmmo_level_<skill>%` in a condition without PlaceholderAPI |
| uxmLang | One language choice for every UXM plugin |

## Installing

1. Drop `uxmShop-<version>.jar` into `plugins/` and remove any older jar.
2. Start the server. The first start fetches the MariaDB and PostgreSQL drivers into the server's `libraries/` folder,
   so it needs network access once.
3. Edit `shops/` and `currencies.conf`, then run `/uxmshop reload`.

## The files

```
plugins/uxmShop/
├── config.conf          language, storage, shop, player-shops, blacklist, menu, log, editor, effects
├── commands.conf        the name, aliases and switch of every command
├── currencies.conf      the currencies and how an amount is written
├── shops/               one catalogue section per file: blocks.conf, tools.conf
├── menus/               hub, section, quantity, sell, shops, playershop
├── messages/            messages_<language>.conf: en, tr, de, es, fr, pt, ru
├── languages.conf       the language each player chose, written by the plugin
├── theme.conf           this plugin's colours on top of plugins/uxmTheme/theme.conf
├── backups/             config.conf as it was before an update added keys
└── data/shop.db         the SQLite database
```

A file that already exists is never overwritten.

## Updating

Replace the jar and restart. When a release adds a key to `config.conf`, the start adds it with its comment and keeps
every value you wrote; the file as it was goes into `backups/`. A line of a language file nobody changed is corrected
to the new wording, and the console lists which. A database file an older version left in the plugin folder moves into
`data/`. The tables are upgraded on start.

## Checking the install

`/uxmshop doctor` checks the storage, the windows and whether the placeholders were published, one line each. The
console lists every problem in the catalogue at start and on every reload.

## Troubleshooting

- **A section is missing.** The file was refused: a line that pays more than it charges, a sold command line, or an
  unknown item field. The console names the file and the line.
- **A player shop refuses a currency.** It cannot pay an owner who is away; see [Currencies](../features/currencies.md).
- **A price reads "not ready".** The currency's plugin is not installed or `enabled = false`.
- **`/shopedit` does not exist.** `editor.enabled` is false.
- **A renamed command is unchanged.** `commands.conf` is read at start only.

Related: [Concepts](concepts.md), [config.conf](../config/config-conf.md)
