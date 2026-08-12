---
title: NPCs
order: 9140
description: Packet-rendered characters with skins, click action chains, nameplates and player ownership.
---

An NPC is an interactive character you place around the server: a shopkeeper at spawn, a warp guide, a tutorial
helper. They are rendered by our own packet layer, so an NPC is not a real world entity: it exists only in each
viewer's client, costs the server almost nothing, and never wanders off or gets killed. Every NPC is edited live
from `/npc` and persisted to the database.

Module `npc` · enabled by default · `modules/npc/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/npc` | Create and manage fake-player NPCs. | `uxmessentials.npc.admin` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.module.npc` | op | Hot-reload / inspect the npc module (server-wide fake-player NPCs behind /npc). |
| `uxmessentials.npc.action` | everyone | /npc command and /npc action: change what an NPC runs when clicked. |
| `uxmessentials.npc.admin` | op | /npc to create, delete, list, move, re-skin, and bind the click command of fake-player NPCs. |
| `uxmessentials.npc.appearance` | everyone | /npc skin, skinslim, type, equip, glow, pose, scale and displayname: change how an NPC looks. |
| `uxmessentials.npc.create` | everyone | /npc create and /npc copy. Held by default alongside the admin node; negate it to leave an operator editing only the NPCs that already exist. |
| `uxmessentials.npc.delete` | everyone | /npc delete. Held by default alongside the admin node; the capability most worth negating for build staff. |
| `uxmessentials.npc.edit` | everyone | Every remaining /npc setting (data, state, cooldown, mirror, collidable, showintab, view and turn distance). |
| `uxmessentials.npc.gui` | op | /npc (no args) opens the NPC management GUI. |
| `uxmessentials.npc.limit.<n>` | op | How many NPCs you may own; the largest tier held wins. |
| `uxmessentials.npc.move` | everyone | /npc movehere, moveto, teleport, center and fix: change where an NPC stands. |
| `uxmessentials.npc.view` | everyone | /npc list, info, nearby and help: read-only inspection. |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `default-limit` | `-1` | How many NPCs a player may own (an NPC is owned by whoever created it). -1 means unlimited; the default, so a server that grants uxmessentials.npc.admin to trusted staff is uncapped as before. Set a number to cap players who lack a uxmessentials.npc.limit.\<n> node; the highest such node a player holds overrides this default, and a uxmessentials.npc.limit.-1 node grants them unlimited. Server/console-created NPCs have no owner and never count. |
| `blocked-commands` | `[]` | Commands an NPC's bound click command or click-action command effect may NOT run, matched case-insensitively on the command's first word (a leading slash is ignored). Empty by default; nothing is blocked. This guards a player-owned NPC from invoking dangerous operator commands once /npc is granted beyond full operators. Example: blocked-commands = ["op", "stop", "deop", "gamemode"] |
| `look-at-player.range` | `12.0` | How close a player must be (in blocks, same world) before a looking NPC turns to face them. Beyond this the NPC keeps the fixed facing it was placed with. |
| `look-at-player.tick-period` | `3` | How often the look loop re-aims every looking NPC at its nearby viewers, in ticks (20 ticks is one second). Lower is smoother but sends more rotation packets; 3 ticks (150 ms) reads as a natural head-turn. |
| `click-cooldown-millis` | `500` | The per-player-per-NPC cooldown (milliseconds) that swallows a duplicate click so a single right-click runs the bound command once, not twice (the client reports the main and off hand separately, and a held button repeats). 0 disables the cooldown. |
| `skin.mineskin-api-key` | `""` |  |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_npc_limit%` | How many NPCs the player may own, or unlimited. |
| `%uxmessentials_npc_owned%` | How many NPCs the player owns. |
| `%uxmessentials_npc_remaining%` | How many more NPCs the player may create, or unlimited. |
| `%uxmessentials_npc_total%` | How many NPCs the server holds. |
{/* /generated */}

## Notes

- **The default type is a player NPC,** a fake player with a skin and a tab entry. Any living entity or display
  entity works too, and `type` changes it later without recreating the NPC.
- **A skin comes from a name, a player profile, a URL or a raw texture,** and `skinslim` picks the classic or
  slim arm model. Name lookups work on offline-mode servers because the fetch does not depend on the joining
  player's own profile.
- **Clicks run the same ordered action chain holograms use,** with a per-NPC cooldown so a chain cannot be
  spammed.
- **A multi-line nameplate is a linked hologram** rather than an NPC property, which is what lets a rank line
  sit above a title line.
- **Players can own NPCs.** `default-limit` caps how many, and `blocked-commands` is what stops an owned NPC's
  action chain running a staff command.

Related: [Holograms](holograms.md), [Villagers](villagers.md), [Menus](../menus/engine.md)
