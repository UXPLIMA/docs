---
title: Configuration
order: 60
description: Every file the plugin writes, what belongs in it, reloading, and what an update does to your files.
icon: settings
---

The plugin writes its files into `plugins/uxmCosmetics/` on first start; the folder is shown in
[Installation](../getting-started/installation.md#the-files). Every file is HOCON. Run `/uxmcosmetics reload`
(`uxmcosmetics.admin.reload`) after an edit.

| Page | What it holds |
|---|---|
| [config.conf](config-conf.md) | Language, storage, permissions, animation, economy, opener, gadget, cleanup, menus, stations, effects |
| [Category files](category-files.md) | `categories/<family>.conf`: every key a piece takes |
| [boxes.conf](boxes-conf.md) | The mystery boxes |
| [stations.conf](stations-conf.md) | The box stations, written by the plugin |
| [commands.conf](commands-conf.md) | The name, aliases and switch of every command |
| [Menu files](menu-files.md) | `menus/`: the four windows |
| [Messages](messages.md) | `messages/messages_<language>.conf`: every line the plugin writes |
| [Languages](languages.md) | The seven languages, adding one, and how a player's language is chosen |
| [theme.conf](theme.md) | The colours, glyphs and small capitals every UXM plugin shares |

## HOCON in two minutes

| Rule | Example |
|---|---|
| A block opens and closes with braces | `opener { slot = 8 }` |
| `=` or `:` between key and value; a block needs neither | `price = 1200.0`, `offset { ... }` |
| Text in quotes, numbers and `true`/`false` without | `rarity = "rare"`, `lead = true` |
| Lists in square brackets, commas or new lines between | `clicks = ["right"]` |
| A comment starts with `#` | `# 0 means not for sale` |

## Reloading

`/uxmcosmetics reload` reads `config.conf`, every family file, `boxes.conf`, `stations.conf`, the windows, the
messages and `theme.conf` again, swaps them in one go, and puts the station signs up again. A cosmetic or box that does
not parse is dropped with its name in the console and the rest load; a file that does not parse stops the reload with
`admin.reload-failed`, and the server keeps what worked.

| Needs a restart | Why |
|---|---|
| `storage` | The database is opened once |
| `animation` | The shared task is made once |
| `permissions.wildcard` | The permission check is made once |
| `commands.conf` | The server builds its command tree once |

## What an update does

1. `config.conf`, `boxes.conf`, `stations.conf`, `commands.conf`, every family file and every window file the jar
   ships that is missing is written.
2. A key a release added is written into your `config.conf` with its comment, at its default.
3. A value you set is never changed.
4. Your file as it was goes to `backups/`; the last five copies are kept.

The merge writes the file again from what it holds, so alignment inside a block may move, and a key you deleted comes
back at its default.

Related: [config.conf](config-conf.md), [Installation](../getting-started/installation.md)
