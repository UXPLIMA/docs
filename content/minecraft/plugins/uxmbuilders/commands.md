---
title: Commands
order: 260
description: One root command, the player subcommands, and the admin tree.
icon: terminal
---

One command: `/builders`. Aliases `/uxmb`, `/bld`, `/insaat`. On its own it opens the main menu.
`/builders help` lists what the sender may run, with descriptions from the language files.

## Players

| Command | Does | Node |
|---|---|---|
| `/builders menu` | Open the main menu | `uxmbuilders.use` |
| `/builders shop [category]` | Open the shop | `uxmbuilders.command.shop` |
| `/builders preview <package>` | Preview a package without a blueprint; no argument closes it | `uxmbuilders.command.preview` |
| `/builders rotate <0\|90\|180\|270>` | Rotate the open preview | `uxmbuilders.command.preview` |
| `/builders mirror` | Mirror the open preview | `uxmbuilders.command.preview` |
| `/builders offset <x> <y> <z>` | Shift the preview, up to 5 per axis | `uxmbuilders.command.preview` |
| `/builders start <package> [rotation]` | Start a build where you look, paying the price | `uxmbuilders.command.start` |
| `/builders list` | Blueprints in your inventory | `uxmbuilders.command.list` |
| `/builders status [id]` | Your running builds | `uxmbuilders.command.status` |
| `/builders pause [id]` | Pause a build | `uxmbuilders.command.pause` |
| `/builders resume [id]` | Resume it | `uxmbuilders.command.pause` |
| `/builders cancel [id]` | Cancel it, with confirmation | `uxmbuilders.command.cancel` |
| `/builders confirm` | Confirm a pending cancel | `uxmbuilders.use` |
| `/builders crew [select\|buy\|upgrade] <builder>` | Manage builders; no argument opens the menu | `uxmbuilders.command.crew` |
| `/builders warehouse` | Open the warehouse | `uxmbuilders.command.warehouse` |
| `/builders warehouse deposit [all]` | Store the held stack, or every placeable item | `uxmbuilders.command.warehouse` |
| `/builders warehouse give <owner> [all]` | Stock a trusting player's warehouse | `uxmbuilders.command.warehouse` |
| `/builders trust <player>`, `trust remove <player>`, `trust list` | Trusted players | `uxmbuilders.command.trust` |
| `/builders boost [id]` | Buy a speed boost | `uxmbuilders.command.boost` |
| `/builders history [player]` | Finished builds; another player's needs `uxmbuilders.history.others` | `uxmbuilders.command.history` |

With one running build the `[id]` can be left out. `pause`, `resume` and `boost` also take the id of a
build whose owner [trusts](features/trusted-players.md) you; `cancel` does not.

The menu buttons check the same nodes as the commands, so denying `uxmbuilders.command.cancel` also
disables the cancel button.

## Admin

Everything below needs `uxmbuilders.admin`, or the single action's node `uxmbuilders.admin.<action>`.
Most also run from the console.

| Command | Does |
|---|---|
| `/builders admin reload` | Re-read every file, clear the schematic cache, re-register permissions |
| `/builders admin save <name> [--overwrite]` | Save your WorldEdit selection as `schematics/<name>.schem` |
| `/builders admin package create <id> <schematic> <price>` | Add a package |
| `/builders admin package edit <id> <field> <value>` | Change one field |
| `/builders admin package info <id>` | Size, block count and materials |
| `/builders admin package list` | Every package |
| `/builders admin package delete <id>` | Remove one |
| `/builders admin give <player> <package> [amount]` | Hand out free blueprints, up to 2,304 |
| `/builders admin build <package> <x> <y> <z> [world] [rotation]` | Build for free, ignoring protection and rules |
| `/builders admin cancel <player\|id\|all>` | Cancel with a full refund |
| `/builders admin setbuilder <player> <builder> [level]` | Set a player's builder level |
| `/builders admin protection` | Which protection providers are active |
| `/builders admin cache [clear]` | The schematic cache |
| `/builders admin perf` | Load factor, blocks per second, tick time and every job |
| `/builders admin npc [killall]` | List builder NPCs, or remove them all while builds continue |
| `/builders admin debug [on\|off\|dump]` | Verbose logging |
| `/builders admin migrate sqlite-to-mysql` | Copy the SQLite data into MySQL; see [The database](config/database.md) |

`save` needs a player with a WorldEdit selection. `admin build` from the console builds on behalf of the
first online player.

Related: [Permissions](permissions.md)
