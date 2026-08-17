---
title: Installation
order: 102
description: Requirements, the first start, and the files that appear.
icon: download
---

## Requirements

| | |
|---|---|
| Server | Bukkit, Spigot or Paper, 1.16 or newer |
| Database | SQLite, bundled (nothing to install) |

## Optional plugins

Every one of these is a `softdepend`. Each is also a switch under `settings.Hooks` in `config.yml`, so
installing the plugin is not enough: the hook must be on.

| Plugin | Hook key | Adds | Default |
|---|---|---|---|
| Vault | `MONEY` | Money as a requirement | **on** |
| PlayerPoints | `PLAYERPOINTS` | Points as a requirement | off |
| CoinsEngine | `COINSENGINE` | CoinsEngine currencies | off |
| PlaceholderAPI | `PLACEHOLDER` | Condition requirements, and placeholders | **on** |
| MMOItems | `MMOITEMS` | MMOItems as requirements and rewards | off |
| EcoItems | `ECOITEM` | EcoItems | off |
| ExecutableItems | `EXECITEM` | ExecutableItems | off |
| ItemsAdder | `IAITEM` | ItemsAdder items | off |
| Nexo | `NEXO` | Nexo items | **on** |
| NBTAPI | n/a | Deeper item matching | n/a |
| n/a | `SAVE` | The built-in binary item store | **on** |
| n/a | `SKULL` | Player heads as items | **on** |
| n/a | `XP` | Vanilla experience as a currency | **on** |

`IAITEM` and `NEXO` are interchangeable. A trade configured with `IAITEM` will use Nexo if only Nexo
is installed, and the reverse.

## Installing

1. Drop the jar into `plugins/`.
2. Start the server once. The plugin writes its files and creates `database.db`.
3. Stop the server.
4. Turn on the hooks you need under `settings.Hooks` in `config.yml`.
5. Define your categories, then edit the trade files they point at.
6. Start again.

## The files

```
plugins/uxmBlacksmith/
├── config.yml          settings, hooks, burn, vanilla crafting, progression, categories
├── language.yml        every message, English and Turkish
├── gui.yml             every menu layout
├── modules/
│   └── boosts.yml      boost item definitions
├── categories/
│   └── *.yml           the trades, one file per category
├── binary_files/       saved items, do not edit by hand
└── database.db         SQLite
```

Each has a page under [Configuration](../config/).

`categories/` files are **generated**. Name a file in a category's `trades:` key, restart, and the
plugin writes it with worked defaults if it does not exist.

<Callout type="danger" title="Never edit binary_files by hand">

Those are serialised `ItemStack`s written by `/blacksmith binary save`. They are not meant to be
readable, and a hand edit produces an item that fails to deserialise, which takes the trade
referencing it down with it. Manage them through the `binary` subcommands.

</Callout>

## Reloading

`/blacksmith reload` re-reads `config.yml`, `language.yml`, `gui.yml`, the boost module and every
category file, and clears the burn permission cache.

Most settings apply without a restart. The database connection does not.

## First checks

| Check | How |
|---|---|
| The plugin loaded | `/blacksmith` opens the main menu |
| Slots are granted | A player without `uxmblacksmith.slot.1` has **zero** slots |
| A hook is live | Configure a requirement using it and open the trade |
| Progression is on | `/blacksmith admin profile <player>` prints a level |
