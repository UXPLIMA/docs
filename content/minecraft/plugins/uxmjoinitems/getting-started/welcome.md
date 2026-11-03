---
title: Welcome
order: 201
description: What uxmJoinItems does, who it is for, and where to read next.
icon: hand
---

A lobby hotbar is a handful of items that open things: a server selector, a cosmetics wardrobe, a lobby list.
uxmJoinItems hands them out when a player joins, joins for the first time, respawns or changes world, keeps them where
they belong, and runs what a click asks for.

## What it does

| Part | What it gives you |
|---|---|
| [Items](../features/items.md) | One file per item: material, slot, name, lore, model, shimmer |
| [Triggers and events](../features/triggers-and-events.md) | Which moments hand an item out, and what each moment runs besides |
| [Clicks](../features/clicks.md) | Left and right click actions, with `[server]` and `[menu]` of this plugin's own |
| [Locking](../features/locking.md) | Items that cannot be moved, dropped or lost on death |
| [Guards and regions](../features/guards-and-regions.md) | Holding an item back while a player is busy, or outside a WorldGuard region |
| [Cooldowns](../features/cooldowns.md) | One click per cooldown, so a held button does not flood the proxy |

Every line a player reads is in a language file, every item is a file you can copy, and the plugin needs nothing else
installed.

## Read next

- [Installation](installation.md): the jar and the files.
- [Concepts](concepts.md): items, triggers, the family rule for a held item.
- [Your first item](first-item.md): a profile item that runs a command.

Related: [Commands](../commands/overview.md), [Item files](../config/item-files.md)
