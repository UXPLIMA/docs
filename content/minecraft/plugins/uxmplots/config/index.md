---
title: Configuration
order: 50
description: Every file the plugin writes, reloading, and what an update does to your files.
icon: settings
---

| Page | What it holds |
|---|---|
| [config.conf](config-conf.md) | Language, storage, plots, limits, work, snapshots, visits, guestbook, tips, sessions, expiry, reports, boards, following, archive, personality, merging, crash items, featured rows, slots, menu sounds |
| [protection.conf](protection-conf.md) | Roads, refusals, forbidden blocks, redstone, cross-boundary |
| [rating.conf](rating-conf.md) | How ratings and trending count |
| [contests.conf](contests-conf.md) | The boss bar and the vote reward |
| [network.conf](network-conf.md) | Servers hearing from each other |
| [World files](world-files.md) | `worlds/<world>.conf` |
| [Contest files](contest-files.md) | `contests/<id>.conf` |
| [Catalogues](catalogues.md) | `catalogues/shop.conf`, `personality.conf`, `tags.conf` |
| [currencies.conf](currencies-conf.md) | What prices are paid in |
| [levels.conf](levels-conf.md) | The level curve, weights and unlocks |
| [discord.conf](discord-conf.md) | Webhook lines |
| [hologram.conf](hologram-conf.md) | The sign over a plot |
| [text-input.conf](text-input-conf.md) | How typing is asked for |
| [commands.conf](commands-conf.md) | The name, aliases and switch of every command |
| [Menu files](menu-files.md) | The fifteen windows and the two editor layouts |
| [Messages](messages.md) | `messages/messages_<language>.conf` |
| [Languages](languages.md) | The seven languages and how a player's language is chosen |
| [theme.conf](theme.md) | The colours, glyphs and small capitals every UXM plugin shares |

Every file is HOCON: `key = value`, blocks in braces, `#` comments.

## Reloading

`/uxmplots reload` reads the files again: config.conf and its four feature files, the worlds, contests, catalogues,
currencies, levels, Discord, the sign, typing, windows and messages. Each swaps its whole snapshot in one go, so a
reader never sees half a file. `storage` and `commands.conf` need a restart. A world's grid (`plot-size`, `road-size`,
`origin`, `bottom-y`, `layers`, `walls`, `road-material`) is written by the generator and is not changed once the world
exists.

## What an update does

When a release adds a key, the next start adds it to `config.conf`, `protection.conf`, `rating.conf`, `contests.conf`
and `network.conf` with its comment, and never changes a value you wrote. The file as it was is kept in `backups/`. A
block an earlier release kept in `config.conf` is moved out into its feature file once.

Related: [config.conf](config-conf.md)
