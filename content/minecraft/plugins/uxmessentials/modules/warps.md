---
title: Warps
order: 9330
description: Server-wide destinations staff create, with costs, locks, passwords, ratings and signs.
---

A warp is a public destination staff create: a spawn hub, an arena, a market, an event stage. `/warp` is the
whole surface, using and managing both, and the older `/setwarp`, `/delwarp` and `/warpinfo` are folded in as
subcommands. Warps live in the database with their owner, creation time, cost, lock and password state, rating
and visitor count.

Module `warps` · enabled by default · `modules/warps/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/warp` (`/wp`, `/warps`) | Teleport to a server warp. | `uxmessentials.warp.use` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.module.warps` | op | Hot-reload / inspect the warps module (server warps and their access rules). |
| `uxmessentials.warp.bypass.lock` | op | Use a locked warp regardless of its lock state. |
| `uxmessentials.warp.bypass.password` | op | Use a password-protected warp without entering its password. |
| `uxmessentials.warp.bypass.safety` | op | Use a warp whose destination fails the safety check. |
| `uxmessentials.warp.delete` | op | /delwarp \<name> to remove a warp. |
| `uxmessentials.warp.edit` | op | /warp editor \<name> to open the warp editor (cost, gates, effects, welcome message, icon). |
| `uxmessentials.warp.info` | everyone | /warpinfo \<name> to show a warp's owner, creation time and cost. |
| `uxmessentials.warp.list` | everyone | /warps to list the warps you may use. |
| `uxmessentials.warp.lock` | op | /warp lock \<name> to lock or unlock a warp against use. |
| `uxmessentials.warp.move` | op | /movewarp \<name> to move an existing warp to your current location. |
| `uxmessentials.warp.others` | op | /warp \<name> \<player> to send another player to a warp. |
| `uxmessentials.warp.password` | op | /warp password \<name> to set or clear a warp's access password. |
| `uxmessentials.warp.set` | op | /setwarp \<name> to create or move a server-wide warp. |
| `uxmessentials.warp.sign.create` | op | Create a [warp] sign that teleports players to a warp on click. |
| `uxmessentials.warp.sign.use` | everyone | Use a [warp] sign to teleport to its warp. |
| `uxmessentials.warp.use` | everyone | /warp \<name> to teleport to a server warp. |
| `uxmessentials.warp.use.<warp>` | everyone | Use one warp, when that warp is configured to require a permission. |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `list-display` | `"gui"` | how bare /warps presents the list: "gui" opens the browse menu, "chat" prints the chat list |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_warp_<warp>_cost%` | What one warp charges to use. |
| `%uxmessentials_warp_<warp>_owner%` | Who created one warp. |
| `%uxmessentials_warp_<warp>_visits%` | How many times one warp has been used. |
| `%uxmessentials_warp_<warp>_world%` | The world one warp sits in. |
| `%uxmessentials_warp_<warp>_x%` | The block x of one warp. |
| `%uxmessentials_warp_<warp>_y%` | The block y of one warp. |
| `%uxmessentials_warp_<warp>_z%` | The block z of one warp. |
| `%uxmessentials_warps_count%` | How many warps the player may use. |
| `%uxmessentials_warps_list%` | The names of the warps the player may use, comma separated. |
{/* /generated */}

## Notes

- **Everything past a bare teleport point is set in `/warp editor`:** the cost, the arrival effects, the welcome
  message, the icon shown in the list and the state gates. The config file holds almost nothing, because a warp
  carries its own settings.
- **Three gates stack:** a cost charged on use, a lock that closes the warp to everyone, and a password you hand
  out. Each has its own bypass node, and `/warp password <name>` with no password clears it.
- **Lock rather than delete for a temporary closure.** Deleting throws away the cost, effects, welcome, rating
  and visitor count with it.
- **`uxmessentials.warp.use` reaches every warp** unless the warp is configured to require its own
  `uxmessentials.warp.use.<warp>` gate, which is how a rank-locked warp is built.
- **Warp costs are command costs**, so `uxmessentials.economy.bypasscmdcost` waives them rather than a
  warp-specific node.
- **A `[warp]` sign teleports whoever clicks it**, which is how a physical warp hub is built. Creating and using
  a sign are separate nodes.
- **An anchor that has become unsafe is refused** for normal players; `uxmessentials.warp.bypass.safety` lets
  staff through to fix it.

Related: [Player Warps](playerwarps.md), [Teleport](teleport.md), [Homes](homes.md)
