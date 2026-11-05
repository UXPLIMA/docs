---
title: Staff
order: 207
description: Reload, doctor, schematics, restore, freezing, languages, and every /isadmin verb.
icon: shield
---

## Under /is

| Command | What it does | Permission |
|---|---|---|
| `/is reload` | Reads the theme, the language files, the menus and the OneBlock phases again | `uxmskyblock.admin.reload` or `uxmskyblock.admin.manage` |
| `/is doctor` | One line each for the database, the windows, the placeholders, the island world and the economy | `uxmskyblock.admin.doctor` |
| `/is schematic pos1 \| pos2` | Marks a corner where you stand | `uxmskyblock.admin.schematic` or `uxmskyblock.admin.manage` |
| `/is schematic save <name>` | Saves the marked box around the block you stand on | same |
| `/is schematic paste <name> [0 \| 90 \| 180 \| 270]` | Pastes a schematic where you stand, turned | same |
| `/is schematic list` | Names the saved schematics | same |
| `/is restore <backup> [mode]` | Restores an island from a backup; the modes are below | `uxmskyblock.admin.restore` |
| `/is restore database <backup> [code]` | Restores the whole database from a backup: without a code it gives you one, and typed again with it, it restores | `uxmskyblock.admin.restore` |
| `/is admin freeze <target> [reason]` | Freezes an island: nobody builds, visitors are sent away | `uxmskyblock.admin.freeze` |
| `/is admin unfreeze <target>` | Lifts the freeze | `uxmskyblock.admin.freeze` |
| `/is admin inspect <target>` | Enters a frozen island to look | `uxmskyblock.admin.inspect` |
| `/is admin inactivity scan` | Looks for islands whose team stopped playing now, rather than at the next run | `uxmskyblock.admin.manage` |
| `/is admin delete <target>` | Deletes an island | `uxmskyblock.admin.manage` |
| `/is admin backup <island>` | Takes a backup of one island | `uxmskyblock.admin.restore` |
| `/is admin backup database` | Takes a backup of the whole database | `uxmskyblock.admin.restore` |
| `/is admin rollback <island> [mode]` | Puts an island back from its newest finished backup | `uxmskyblock.admin.restore` or `uxmskyblock.admin.manage` |
| `/is admin restore <backup> [mode]` | The same as `/is restore` | `uxmskyblock.admin.restore` or `uxmskyblock.admin.manage` |
| `/is lang set-player <player> <language>` | Sets the language another player reads | `uxmskyblock.admin.language` |
| `/is lang reset-player <player>` | Clears it | `uxmskyblock.admin.language` |

Restore modes:

| Mode | Puts back |
|---|---|
| `GEOMETRY_ONLY` | Blocks and biomes only |
| `WORLD_CONTENT_SAFE` | The world, where the island is and the flags that protect it; membership is left alone |
| `FULL_ISLAND` | The world, the island's place and flags, its members, roles and upgrades |

With no mode a restore uses `WORLD_CONTENT_SAFE`.

`/is reload` does not read `config.conf`, `modules.conf`, `commands.conf` or the module files; those need a restart.

## /isadmin

Each verb asks its own node. `<target>` is a player's name or an island id.

| Command | What it does | Permission |
|---|---|---|
| `/isadmin oneblock setphase <target> <phase>` | Moves a OneBlock island to a phase | `uxmskyblock.admin.oneblock.setphase` |
| `/isadmin oneblock setcount <target> <count>` | Sets a OneBlock island's count of breaks | `uxmskyblock.admin.oneblock.setcount` |
| `/isadmin oneblock addcount <target> <count>` | Adds to or takes from it | `uxmskyblock.admin.oneblock.addcount` |
| `/isadmin oneblock check <target>` | Reads where a OneBlock island stands | `uxmskyblock.admin.oneblock.check` |
| `/isadmin oneblock respawn <target>` | Lays the block again | `uxmskyblock.admin.oneblock.respawn` |
| `/isadmin chunkblock open <target> [chunk-x chunk-z]` | Opens a chunk of a ChunkBlock island for good; the console names the chunk | `uxmskyblock.admin.chunkblock.open` |
| `/isadmin chunkblock close <target> [chunk-x chunk-z]` | Closes a chunk | `uxmskyblock.admin.chunkblock.close` |
| `/isadmin chunkblock reset <target>` | Closes every chunk but the first | `uxmskyblock.admin.chunkblock.reset` |
| `/isadmin tradewinds setrank <target> <rank>` | Gives a vessel a rank | `uxmskyblock.admin.tradewinds.setrank` |
| `/isadmin tradewinds setstanding <target> <port> <standing>` | Sets a vessel's standing in a port | `uxmskyblock.admin.tradewinds.setstanding` |
| `/isadmin tradewinds moveport <target> <port>` | Puts a vessel in a port at once | `uxmskyblock.admin.tradewinds.moveport` |
| `/isadmin realms remirror <target> [chunk-x chunk-z]` | Lays the Upside Down again from the island's land | `uxmskyblock.admin.realms.remirror` |
| `/isadmin parkour resettimes <target> [runner]` | Forgets the runs on a course, of everyone or one runner | `uxmskyblock.admin.parkour.resettimes` |
| `/isadmin brix startcontest [theme]` | Opens a build contest on a theme | `uxmskyblock.admin.brix.startcontest` |
| `/isadmin brix stopcontest` | Takes the contest off, with its entries and votes | `uxmskyblock.admin.brix.stopcontest` |
| `/isadmin loot mark <table>` | Fills the container you look at from a loot table the first time a player opens it | `uxmskyblock.admin.loot.mark` |
| `/isadmin top add <board> <rank> [sign \| head \| text]` | Makes what you look at show a place of a leaderboard | `uxmskyblock.admin.top.add` |
| `/isadmin top remove` | Takes away the display you look at or stand on | `uxmskyblock.admin.top.remove` |
| `/isadmin top list` | Lists the displays | `uxmskyblock.admin.top.list` |
| `/isadmin boxed setsize <target> <size>` | Sets how far across a box is | `uxmskyblock.admin.boxed.setsize` |
| `/isadmin boxed grow <target> <blocks>` | Pushes a box out | `uxmskyblock.admin.boxed.grow` |
