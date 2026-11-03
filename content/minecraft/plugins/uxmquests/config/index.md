---
title: Configuration
order: 150
description: Every file the plugin writes, reloading, and what an update does to your files.
icon: settings
---

| Page | What it holds |
|---|---|
| [config.conf](config-conf.md) | Language, storage, overall milestones, the battle pass, anti-glitch, feedback, sampling, ranks, window sounds and phases |
| [Group files](group-files.md) | `groups/<id>.conf` |
| [Pass files](pass-files.md) | `passes/<id>.conf` |
| [commands.conf](commands-conf.md) | The name, aliases and switch of every command |
| [Menu files](menu-files.md) | The four windows |
| [Messages](messages.md) | `messages/messages_<language>.conf` |
| [Languages](languages.md) | The seven languages and how a player's language is chosen |
| [theme.conf](theme.md) | The colours, glyphs and small capitals every UXM plugin shares |

Every file is HOCON: `key = value`, blocks in braces, `#` comments.

## Reloading

`/uxmquests reload` reads `config.conf`, the groups, the passes, the windows, the messages and the theme again, and is
applied as a whole or not at all. A group file that does not parse leaves the running groups as they were; the console
names the file and the reload says the groups were kept. `/uxmquests problems` lists what the last load could not read.
`storage` and `commands.conf` need a restart.

## What an update does

When a release adds a key to `config.conf`, the next start adds it with its comment and never changes a value you
wrote. The file as it was is kept in `backups/`. Alignment inside a block may move. A line in a shipped
`messages_<language>.conf` that nobody edited takes the wording of the new release.

Related: [config.conf](config-conf.md)
