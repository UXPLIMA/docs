---
title: Welcome
order: 890
description: What uxmEssentials is, what it replaces, and where to go next.
---

uxmEssentials is one jar that covers what a survival server usually installs a dozen plugins for: homes,
warps, teleports, an economy, kits, vaults, moderation, staff tools, holograms, NPCs, a scoreboard and
tablist, a world manager and a menu engine. It runs on Paper 26.1.2 and on Folia.

The plugin is 34 modules. Each one owns its commands, permissions and config file, and each can be turned
off: a disabled module registers nothing and runs no migrations. 25 ship on, 9 ship off. See
[Modules](../modules/index.md) for the list.

Everything a player sees lives in text files. Commands are renamed in `commands/`, messages in `messages/`,
menus in `menus/`, and every module keeps its settings in `modules/<module>/config.conf`. All of it is
HOCON, and all of it reloads with `/uxmess reload`.

## Where to start

| Page | What it covers |
|---|---|
| [Installation](installation.md) | Requirements, the first start, and how to check the install |
| [Core Concepts](concepts.md) | Modules, config, permissions, messages |
| [Modules](../modules/index.md) | One page per module, with its commands and settings |
| [Migrating from EssentialsX](migration.md) | Bringing homes, warps, balances and kits across |
| [Configuration](../config/overview.md) | The config tree and how reloads behave |

## What it is not

uxmEssentials does not replace a permission plugin, a land-claim plugin or a chat-channel plugin. It reads
LuckPerms when present, respects the claim plugins you run, and formats chat without owning channels. See
[Integrations](../integrations/overview.md) for what it detects and what each detection turns on.
