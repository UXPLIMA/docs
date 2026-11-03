---
title: Configuration
order: 150
description: Every file the plugin writes, reloading, and what an update does to your files.
icon: settings
---

| Page | What it holds |
|---|---|
| [config.conf](config-conf.md) | Language, storage, tick, render, placeholders, fuel switch, experience, destinations, prices, economy, banking, loops, smelting, upgrades, limits, claims, teleport, ledger, audit, effects, editor, menu sounds |
| [hologram.conf](hologram-conf.md) | The hologram over a minion |
| [tool.conf](tool-conf.md) | Tool wear and tool enchantments |
| [fuel.conf](fuel-conf.md) | The fuel grades |
| [prices.conf](prices-conf.md) | The seller's own price table |
| [skins.conf](skins-conf.md) | The suits |
| [Type files](../features/types-and-levels.md) | `types/<id>.conf` |
| [commands.conf](commands-conf.md) | The name, aliases and switch of every command |
| [Menu files](menu-files.md) | The fourteen windows |
| [Messages](messages.md) | `messages/messages_<language>.conf` |
| [Languages](languages.md) | The seven languages and how a player's language is chosen |
| [theme.conf](theme.md) | The colours, glyphs and small capitals every UXM plugin shares |

Every file is HOCON: `key = value`, blocks in braces, `#` comments.

## Reloading

`/uxmminions reload` swaps the new files in as one snapshot and lists what still needs a restart: the storage backend
and prefix, `commands.conf`, every interval, `prices.sources` and `prices.conf`, `claims.disabled-providers`,
`ledger.enabled`, and `editor.enabled`.

## What an update does

When a release adds a key, the next start adds it to `config.conf`, `hologram.conf` or `tool.conf` with its comment, and
never changes a value you wrote. Your file as it was is kept in `backups/`; the last five copies are kept. A quota you
removed from `limits.per-type` stays removed.

Related: [config.conf](config-conf.md)
