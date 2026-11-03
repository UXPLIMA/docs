---
title: Configuration
order: 150
description: Every file the plugin writes, what belongs in it, reloading, and what an update does to your files.
icon: settings
---

A value that changes behaviour is in `config.conf`, content is in its own file or module folder, a word a player
reads is in a language file, a window is a file of its own, and a colour of the interface is in the shared theme.

| Page | What it holds |
|---|---|
| [config.conf](config-conf.md) | Language, server name, storage, network, modules, chat rules, limits |
| [commands.conf](commands-conf.md) | The name, aliases and switch of every command |
| [channels.conf](channels-conf.md) | The channels a new database starts with |
| [formats.conf](formats-conf.md) | How a line is drawn |
| [currencies.conf](currencies-conf.md) | The currencies tags are sold in |
| [filters.conf](filters-conf.md) | Filters, the ladder, automatic replies, word lists |
| [emojis.conf](emojis-conf.md) | Emoji |
| [colours.conf](colours-conf.md) | Chat colours |
| [tags.conf](tags-conf.md) | Tag categories, rarities, rules and the starting tags |
| [announcements.conf](announcements-conf.md) | The starting announcement bundles |
| [events.conf](events-conf.md) | Server lines |
| [custom-commands.conf](custom-commands-conf.md) | Commands of your own |
| [games.conf](games-conf.md) | Chat games, words and trivia |
| [Menu files](menu-files.md) | The ten windows |
| [Messages](messages.md) | `messages/messages_<language>.conf` |
| [Languages](languages.md) | The seven languages and how a player's language is chosen |
| [theme.conf](theme.md) | The colours, glyphs and small capitals every UXM plugin shares |
| [Velocity config](velocity-conf.md) | The proxy plugin |
| [discord.conf](discord-conf.md) | The Discord add-on |

## Reloading

`/uxmchat reload` reads `config.conf`, `formats.conf`, `currencies.conf`, the module files, the messages and the
menus again and swaps them in whole, so a reload is never half applied. It turns modules on and off. `storage`,
`network`, `commands.conf` and `custom-commands.conf` need a restart. Channels, tags and announcement bundles live in
the database and are changed in game.

## What an update does

When a release adds a key, the next start adds it to your `config.conf` with the comment that explains it, and it
never changes a value you wrote. Your file as it was is kept in `backups/`; the last five copies of each file are
kept. The file is written again from what it holds, so alignment inside a block may move. A key you deleted comes
back at its default, which is why every switch says what turning it off means.

Related: [config.conf](config-conf.md)
