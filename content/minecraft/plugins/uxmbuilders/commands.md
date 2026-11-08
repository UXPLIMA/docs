---
title: Commands
order: 360
description: One root command, the player subcommands, and the admin tree.
icon: terminal
---

One command: `/builders`. Aliases `/uxmb`, `/bld`, `/insaat`. On its own it opens the main menu for a
player and prints the help for the console. `/builders help [page]` (or `?`) lists what the sender may
run, eight to a page, with descriptions from `messages.yml`. Subcommands ignore letter case and
tab-complete only what the sender may use.

## Players

| Command | Does | Node |
|---|---|---|
| `/builders menu` | Open the main menu | `uxmbuilders.use` |
| `/builders shop [category]` | Open the shop, on a category if one is named | `uxmbuilders.command.shop` |
| `/builders preview <package>` | Preview a package without a blueprint; no argument closes it | `uxmbuilders.command.preview` |
| `/builders rotate [0\|90\|180\|270]` | Rotate the open preview; no argument turns it 90 degrees | `uxmbuilders.command.preview` |
| `/builders mirror` (`aynala`) | Mirror the open preview | `uxmbuilders.command.preview` |
| `/builders offset <x> <y> <z>` | Shift the preview, up to 5 per axis | `uxmbuilders.command.preview` |
| `/builders start <package> [rotation]` | Start a build where you look, paying the price now | `uxmbuilders.command.start` |
| `/builders list` | Blueprints in your inventory, with their uses | `uxmbuilders.command.list` |
| `/builders status [id]` | Your running builds, and your slots and builder | `uxmbuilders.command.status` |
| `/builders pause [id]` | Pause a build | `uxmbuilders.command.pause` |
| `/builders resume [id]` | Resume it | `uxmbuilders.command.pause` |
| `/builders boost [id]` | Buy a [speed boost](features/boost.md) | `uxmbuilders.command.boost` |
| `/builders cancel [id]` | Cancel it; asks for `/builders confirm` first | `uxmbuilders.command.cancel` |
| `/builders confirm` (`onayla`) | Confirm a pending cancel | `uxmbuilders.use` |
| `/builders crew` | Open the crew menu | `uxmbuilders.command.crew` |
| `/builders crew select [builder]` | Select an owned builder | `uxmbuilders.command.crew` |
| `/builders crew buy [builder]` | Buy a builder | `uxmbuilders.command.crew` |
| `/builders crew upgrade [builder]` | Buy its next level | `uxmbuilders.command.crew` |
| `/builders warehouse` (`depot`, `wh`) | Open your warehouse | `uxmbuilders.command.warehouse` |
| `/builders warehouse <player>` | Open a window into the warehouse of an owner who trusts you, to stock it | `uxmbuilders.command.warehouse` |
| `/builders warehouse deposit [all]` | Store the held stack, or with `all` every plain block item | `uxmbuilders.command.warehouse` |
| `/builders warehouse give <owner> [all]` | The same, into the warehouse of an owner who trusts you | `uxmbuilders.command.warehouse` |
| `/builders trust <player>`, `trust add <player>` | Trust a player with your builds | `uxmbuilders.command.trust` |
| `/builders trust remove <player>` | Withdraw it | `uxmbuilders.command.trust` |
| `/builders trust`, `trust list` | Who you trust, and who trusts you | `uxmbuilders.command.trust` |
| `/builders history [player]` | Finished and cancelled builds; another player's needs `uxmbuilders.history.others` | `uxmbuilders.command.history` |

With one running build the `[id]` can be left out. `pause`, `resume` and `boost` also take the id of a
build whose owner [trusts](features/trusted-players.md) you; `cancel` does not. A `crew` command without
a builder acts on the selected one.

`cancel` skips the confirmation when `start.confirmCancel` is off, and for a build a restart interrupted,
whose refund is the whole unbuilt share. The confirmation waits `start.confirmTimeout` seconds.

The menu buttons check the same nodes as the commands, so denying `uxmbuilders.command.cancel` also
disables the cancel button.

## Market

The [player market](features/market/index.md). A `<code>` is a listing's six-character code.

| Command | Does | Node |
|---|---|---|
| `/builders market`, `market browse [category]` | Open the market, on a category if one is named | `uxmbuilders.market.use` |
| `/builders market search <words>` | Open it on the listings whose title, description or seller holds the words | `uxmbuilders.market.use` |
| `/builders market view <code>` | Open one listing | `uxmbuilders.market.use` |
| `/builders market preview <code>` | Preview a listing where you look | `uxmbuilders.market.use` |
| `/builders market buy <code>` | Buy it, through the confirmation | `uxmbuilders.market.buy` |
| `/builders market designs` (`library`) | The designs you bought; get a blueprint again | `uxmbuilders.market.use` |
| `/builders market wand` | Get the selection wand | `uxmbuilders.market.wand` |
| `/builders market pos1`, `pos2` | Mark the block you look at as the first or second corner | `uxmbuilders.market.wand` |
| `/builders market clear` | Forget the corners | `uxmbuilders.market.wand` |
| `/builders market sell` | Capture the selection and fill in the listing | `uxmbuilders.market.sell` |
| `/builders market mine` (`listings`) | Your listings and earnings | `uxmbuilders.market.sell` |
| `/builders market earnings` | What the market earned you: paid, owed and held | `uxmbuilders.market.sell` |
| `/builders market claim` | Pay out what you are owed now | `uxmbuilders.market.sell` |
| `/builders market report <code> <reason> [note]` | Report a listing | `uxmbuilders.market.report` |

The market commands need a player; `/builders market` from the console prints its usage. With the
market turned off, `designs`, `earnings` and `claim` still work.

## Admin

Everything below needs `uxmbuilders.admin`, or the single action's node `uxmbuilders.admin.<action>`.
`/builders admin` alone prints the list of actions. Most actions also run from the console.

| Command | Does |
|---|---|
| `/builders admin reload` | Re-read every file; see [Reloading](getting-started/installation.md#reloading) |
| `/builders admin wand` | Give yourself the selection wand: left-click the first corner, right-click the second |
| `/builders admin save <name> [--overwrite]` | Save your selection, the wand's or else WorldEdit's, as `schematics/<name>.schem` |
| `/builders admin package create <id> <schematic> <price>` | Add a package |
| `/builders admin package edit <id> <field> <value>` | Change one field |
| `/builders admin package info <id>` | Size, block count and the eight most used materials |
| `/builders admin package list` | Every package |
| `/builders admin package delete <id>` | Remove one |
| `/builders admin give <player> <package> [amount]` | Hand out free blueprints, up to 2,304, to an online player |
| `/builders admin build <package> <x> <y> <z> [world] [rotation]` | Build for free, ignoring protection, rules, limits and materials |
| `/builders admin cancel <player\|id\|all>` | Cancel with a full refund: every build of an online player, one build, or all of them |
| `/builders admin setbuilder <player> <builder> [level]` | Give a player a builder at a level (1 by default) and select it |
| `/builders admin protection` | Each protection provider, its priority, and whether it is available |
| `/builders admin cache [info\|clear]` | The schematic cache |
| `/builders admin perf` | Load factor, blocks per second, tick time, TPS and every job |
| `/builders admin npc [list\|killall]` | List builder NPCs and their viewers, or remove them all while builds continue |
| `/builders admin debug [on\|off\|dump]` | Turn debug logging on or off until the next restart; no argument toggles it. `dump` prints `perf`, `protection`, `npc` and `cache` at once |
| `/builders admin migrate sqlite-to-mysql` | Copy the SQLite data into MySQL or PostgreSQL; see [The database](config/database.md) |
| `/builders admin market remove <code> [--revoke] [reason]` | Take a listing down; `--revoke` also takes back every licence it sold |
| `/builders admin market restore <code>` | Put a hidden or removed listing back |
| `/builders admin market reports [page]` | The reported listings, the one waiting longest first |
| `/builders admin market resolve <report> dismiss\|action` | Close one report |
| `/builders admin market inspect <code>` | A listing's reports, licences, origin and seller |
| `/builders admin market tp <code>` | Teleport to where a listing was captured |
| `/builders admin market ban <player> [duration] [reason]` | Ban a seller from the market, for a time such as `7d` or for good |
| `/builders admin market unban <player>` | Lift the ban and release the held earnings |
| `/builders admin market refund <purchase> [--revoke]` | Take back one licence and refund its buyer; with `--revoke`, as `moderation.revokeRefund` says |
| `/builders admin market payout <player>` | Pay what the market owes a player now |

`wand`, `save` and `market tp` need a player. `admin build` from the console builds on behalf of the
first online player. `setbuilder` and the `market` actions also find players who are offline but have
played before. With debug on, every build event and every refused command is written to the console.
The `market` actions, all under `uxmbuilders.admin.market`, are explained on
[Market moderation](features/market/moderation.md).

Related: [Permissions](permissions.md)
