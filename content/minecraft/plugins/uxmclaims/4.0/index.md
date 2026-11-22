---
title: uxmClaims 4.0
order: 100
description: Lands, towns and nations in one plugin, built from modules you switch on and off.
icon: land-plot
---

uxmClaims 4 keeps the chunk claims of 3.x and adds towns, nations, banks, taxes, wars and more as
separate modules. One jar holds all of them. You choose which ones run, and the rest stay out of the
way: a module that is off registers no command, no listener and no menu.

## What changed since 3.x

| Area | 3.x | 4.0 |
|---|---|---|
| Shape | One plugin with one feature set | A kernel and about 50 modules, each with its own config, texts, commands and tables |
| Land owners | Players | Players, towns, nations or the server |
| Groups | None | [Towns](modules/towns.md), [Nations](modules/nations.md), [Relations](modules/relations.md), [War](modules/war.md) |
| Money | Costs through Vault | Costs plus [banks](modules/bank.md), [taxes and upkeep](modules/taxes.md), [plots for sale or rent](modules/plots.md) |
| Files | `config.yml`, `messages.yml`, `entitlements.yml` and the rest in one folder | `config.yml` for the kernel, `modules.yml`, and a folder per module under `modules/` |
| Platforms | A bukkit jar and a folia jar | One jar for Spigot, Paper and Folia |
| Languages | Texts per server | Texts per server, or per player when you turn it on |
| Admin bypass | Always on for admins | Kept on for servers moved from 3.x; switched per admin with `/uxm bypass` on new installs (see [Behaviour changes](upgrading/behaviour-changes.md)) |
| API | `com.uxplima.claim.*` | `com.uxplima.uxmclaims.api.*`, with the old API kept working through [Legacy API](modules/legacy-api.md) |

Your 3.x claims, roles, members, warps, vaults and settings move over on the first start of 4.0. The
move can be tried first, and undone. See [Upgrading from 3.x](upgrading/from-3x.md).

## The modular idea

Every feature lives in a module. A module declares what it needs: [Towns](modules/towns.md) needs
[Lands](modules/lands.md), [War](modules/war.md) needs [Relations](modules/relations.md). The
kernel starts the modules in dependency order, and a module whose needs are not met stays off with
the reason in `/uxm modules`. A module that fails at start-up is turned off on its own; the others
keep running and no land loses its protection.

You turn modules on and off in `plugins/uxmClaims/modules.yml` and restart. The full list is on
[Modules](modules/index.md), and the file is explained on [modules.yml](configuration/modules-yml.md).

## Presets

A preset is a ready-made choice of modules and a few values. It writes `modules.yml` and those values
once, and keeps a backup of every file it changes. It never touches lands or storage.

| Preset | For |
|---|---|
| `classic` | The 3.x feature set: personal lands, warps, vaults, claim blocks. A clean install starts here |
| `survival-claims` | The simplest claiming: personal lands, borders and notifications, no claim block or expiry |
| `towny-economy` | Towns, nations, banks, taxes, plots and relations, no personal lands, a locked wilderness |
| `lands-style` | Personal lands with a land bank, member tax, relations, plots and claim points |
| `war-server` | `towny-economy` with PvP values, combat tag, newbie protection, war and rollback |
| `network` | `classic` for several servers on one MySQL, MariaDB or PostgreSQL database |

Pick one in game with `/uxm setup`, or from the console with `/uxm preset apply <id>`. What each one
turns on is on [Presets](configuration/presets.md).

## Where to go next

| You want to | Read |
|---|---|
| Install uxmClaims for the first time | [Installation](getting-started/installation.md) |
| Upgrade a 3.x server | [Upgrading from 3.x](upgrading/from-3x.md) |
| Find a command or a setting | The module's page under [Modules](modules/index.md) |
| Grant permissions | [Permissions](permissions.md) |
| Show uxmClaims values in other plugins | [Placeholders](placeholders.md) |
| Run several servers on one database | [Network mode](network-mode.md) |
| Write an add-on | [Developer](developer/index.md) |
