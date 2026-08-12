---
title: Configuration
order: 114
description: The config tree, the global file, per-module files, command names and messages.
---

Settings are HOCON files under `plugins/uxmEssentials/`. Nothing player-facing is hardcoded.

| Page | What it covers |
|---|---|
| [Overview](overview.md) | The config tree, reload behaviour, what an update does to your files |
| [config.conf](global-config.md) | Storage, locale, network, claims, links and the other globals |
| [Per-Module Config](per-module.md) | The `modules/<module>/config.conf` files and the `enabled` key |
| [Renaming Commands](commands-conf.md) | Renaming, re-aliasing and disabling any command |
| [Messages](messages.md) | Per-language catalogs, MiniMessage, `/lang` |
| [UI Style](ui-style.md) | The palette, glyphs and formats the built-in text follows |
