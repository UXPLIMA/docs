---
title: First start
order: 112
description: The setup menu, choosing a preset, and the first land.
icon: flag
---

A clean install runs the `classic` preset: the 3.x feature set with personal lands. The first start
prints the other presets to the console, and the first admin who joins gets a clickable line that
opens the setup menu. Both are shown once.

## The setup menu

`/uxm setup` (needs `uxmclaims.admin`) opens a list of presets. Picking one opens its module list, with
each module marked on, off or always on. Click a module to change it, then Apply.

| Step | What happens |
|---|---|
| Apply | `modules.yml` and the preset's values are written; a copy of every changed file goes to `backups/` first |
| Restart | Modules turn on or off. No land loses its protection on the way |
| Values only | Values that a module reads at start-up also need the restart; the rest apply at once through `/uxm reload` |

The menu remembers the last preset in `data/setup.yml`, and the hint is not shown again once a choice is
made.

## From the console

| Command | What it does |
|---|---|
| `/uxm preset list` | Lists the presets with a short description |
| `/uxm preset show <id>` | Shows what a preset would change, without changing anything |
| `/uxm preset apply <id>` | Shows the changes and asks for a confirmation |
| `/uxm preset apply <id> --dry-run` | Shows the changes only |
| `/uxm preset confirm` | Applies the preset shown by `apply`, within two minutes |

The dry run warns when a preset needs a plugin you do not have (`towny-economy` needs Vault), a storage
you do not use (`network` needs MySQL, MariaDB or PostgreSQL), or a module whose dependency stays off.
A warning does not stop the preset, so read them before you confirm.
What each preset turns on is on [Presets](../configuration/presets.md).

## Your first land

With `classic`, `survival-claims` or `lands-style`, any player can claim straight away:

1. Stand in a protected world and run `/claim create [name]`. The chunk you stand in becomes the land.
2. Run `/claim` or `/claim menu` to open the land menu: members, roles, flags, chunks and the rest.
3. Add chunks next to it with the chunk commands on [Lands](../modules/lands.md).

With `towny-economy` and `war-server`, personal lands are off. Players found a town instead, and the
town owns its lands: see [Towns](../modules/towns.md).

## Before you open the server

| Check | Where |
|---|---|
| Which worlds are protected, and what the wilderness allows | `modules/lands/worlds.yml`, see [Lands](../modules/lands.md) |
| How many lands and chunks a player may have | [Limits](../modules/limits.md) |
| What claiming costs | [Economy](../modules/economy.md) |
| How admins bypass protection | `protection.admin-bypass` in `modules/lands/config.yml`, see [Behaviour changes](../upgrading/behaviour-changes.md) |
| The language | `language.default` in `config.yml`, see [Languages](../configuration/languages.md) |

Related: [Installation](installation.md), [Presets](../configuration/presets.md), [Modules](../modules/index.md)
