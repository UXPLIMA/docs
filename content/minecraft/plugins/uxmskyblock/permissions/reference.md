---
title: Reference
order: 701
description: Every permission node, its default and what it grants.
icon: key
---

Nodes decide which commands a player may type and which island rules they pass. What a member may do on their own
island is decided by [island roles](../features/roles.md), not by nodes.

<Callout type="warning" title="Bypass nodes are nobody's by default">
The `uxmskyblock.bypass.` nodes default to nobody, not even operators. Give them by hand to the staff who need them.
</Callout>

## Players

Every player holds these. Take one away to close that command.

| Node | Default | What it allows |
|---|---|---|
| `uxmskyblock.island.create` | everyone | Create a new skyblock island |
| `uxmskyblock.island.delete` | everyone | Delete an island |
| `uxmskyblock.island.home` | everyone | Teleport to island home |
| `uxmskyblock.island.sethome` | everyone | Set island home location |
| `uxmskyblock.island.invite` | everyone | Invite members to island |
| `uxmskyblock.island.kick` | everyone | Kick members from island |
| `uxmskyblock.island.ban` | everyone | Ban players from island |
| `uxmskyblock.island.unban` | everyone | Unban players from island |
| `uxmskyblock.island.bank` | everyone | Access island bank |
| `uxmskyblock.island.biome` | everyone | Change island biome |
| `uxmskyblock.island.upgrade` | everyone | Upgrade island perks |
| `uxmskyblock.island.top` | everyone | View island leaderboards |
| `uxmskyblock.island.fly` | everyone | Fly on your own island with /is fly |

## Staff

| Node | Default | What it allows |
|---|---|---|
| `uxmskyblock.admin.bypass` | op | Bypass all island protection and boundaries |
| `uxmskyblock.admin.language` | op | Set or clear the language another player reads |
| `uxmskyblock.admin.reload` | op | Reload plugin configuration |
| `uxmskyblock.admin.doctor` | op | Ask how the plugin is, one line per thing that can be wrong |
| `uxmskyblock.admin.manage` | op | Administrative island management |
| `uxmskyblock.admin.freeze` | op | Freeze and unfreeze islands administratively |
| `uxmskyblock.admin.inspect` | op | Inspect and bypass frozen island quarantines |
| `uxmskyblock.admin.restore` | op | Back up and restore islands and the database |
| `uxmskyblock.admin.booster` | op | Grant island boosters administratively |
| `uxmskyblock.admin.schematic` | op | Save and paste schematics |
| `uxmskyblock.admin.oneblock.setphase` | op | Move a OneBlock island to a phase |
| `uxmskyblock.admin.oneblock.setcount` | op | Set a OneBlock island's count of breaks |
| `uxmskyblock.admin.oneblock.addcount` | op | Add to or take from a OneBlock island's count of breaks |
| `uxmskyblock.admin.oneblock.check` | op | Read where a OneBlock island stands |
| `uxmskyblock.admin.boxed.setsize` | op | Set how far across a Boxed island's box is |
| `uxmskyblock.admin.boxed.grow` | op | Push a Boxed island's box out |
| `uxmskyblock.admin.chunkblock.open` | op | Open a chunk of a ChunkBlock island |
| `uxmskyblock.admin.chunkblock.close` | op | Close a chunk of a ChunkBlock island |
| `uxmskyblock.admin.chunkblock.reset` | op | Close every chunk of a ChunkBlock island but the first |
| `uxmskyblock.admin.tradewinds.setrank` | op | Give a TradeWinds vessel a rank |
| `uxmskyblock.admin.tradewinds.setstanding` | op | Set a TradeWinds vessel's standing in a port |
| `uxmskyblock.admin.tradewinds.moveport` | op | Put a TradeWinds vessel in a port at once |
| `uxmskyblock.admin.realms.remirror` | op | Lay a StrangerRealms island's Upside Down again from its land |
| `uxmskyblock.admin.parkour.resettimes` | op | Forget the runs on a Parkour course, of every runner or of one |
| `uxmskyblock.admin.brix.startcontest` | op | Open a Brix build contest, on a theme written or a configured one |
| `uxmskyblock.admin.brix.stopcontest` | op | Take the Brix build contest under way off, with its entries and votes |
| `uxmskyblock.admin.loot.mark` | op | Give a container a loot table, filled when it is first opened |
| `uxmskyblock.admin.top.add` | op | Make the sign, head or spot looked at show an island of a leaderboard |
| `uxmskyblock.admin.top.remove` | op | Take a leaderboard display away |
| `uxmskyblock.admin.top.list` | op | List the leaderboard displays |
| `uxmskyblock.admin.oneblock.respawn` | op | Lay a OneBlock island's block again |

## Bypasses, game modes and wildcards

| Node | Default | What it allows |
|---|---|---|
| `uxmskyblock.bypass.limits` | nobody | Place past an island's block and creature limits |
| `uxmskyblock.bypass.quarantine` | nobody | Enter a quarantined island |
| `uxmskyblock.bypass.coopcooldown` | nobody | Join another island before the join cooldown ends |
| `uxmskyblock.bypass.resetlimits` | nobody | Reset an island past the reset limits |
| `uxmskyblock.brix.keepmode` | op | Keep one's game mode on a Brix plot |
| `uxmskyblock.parkour.keepmode` | op | Keep one's game mode on a Parkour course |
| `uxmskyblock.parkour.checks.bypass` | nobody | Run a Parkour course without its checks |
| `uxmskyblock.chat.spy` | op | Spy on island private chat channels |
| `uxmskyblock.admin.*` | op | Every `uxmskyblock.admin.` node above |
| `uxmskyblock.bypass.*` | nobody | Every `uxmskyblock.bypass.` node above |
| `uxmskyblock.*` | nobody | Every node above |

The bypass, keep-mode and Parkour check nodes are the names a module file sets; change the name there and the node
changes. A word a player may not use stays out of their command list, and typed anyway it answers with the
no-permission line in their language.
