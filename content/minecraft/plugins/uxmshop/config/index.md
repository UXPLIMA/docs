---
title: Configuration
order: 150
description: Every file the plugin writes, reloading, and what an update does to your files.
icon: settings
---

| Page | What it holds |
|---|---|
| [config.conf](config-conf.md) | Language, storage, the catalogue's rules, player shops, the blacklist, window sounds, the log, the editor switch, trade effects |
| [currencies.conf](currencies-conf.md) | The currencies and how an amount is written |
| [Section files](section-files.md) | `shops/<section>.conf` |
| [commands.conf](commands-conf.md) | The name, aliases and switch of every command |
| [Menu files](menu-files.md) | The six windows |
| [Messages](messages.md) | `messages/messages_<language>.conf` |
| [Languages](languages.md) | The seven languages and how a player's language is chosen |
| [theme.conf](theme.md) | The colours, glyphs and small capitals every UXM plugin shares |

Every file is HOCON: `key = value`, blocks in braces, `#` comments.

## Reloading

`/uxmshop reload` reads the configuration, the currencies, the catalogue, the windows, the messages and the theme
again, as a whole or not at all: when a file does not parse, the plugin keeps what it had and the console says why.
`commands.conf` and `storage` need a restart: the command tree is built once, and the database is opened once.

## What an update does

When a release adds a key to `config.conf`, the next start adds it with its comment and never changes a value you
wrote. The file as it was is kept in `backups/`. A line of a shipped language file that nobody changed is corrected to
the new wording, and the console lists which.

Related: [config.conf](config-conf.md)
