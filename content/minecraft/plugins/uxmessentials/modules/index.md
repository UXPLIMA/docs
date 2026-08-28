---
title: Modules
order: 94
description: Every feature ships as a module you can switch off. One page each.
icon: blocks
---

uxmEssentials is 35 modules behind one jar. Each one owns its commands, its permission nodes, its placeholders
and its config file at `plugins/uxmEssentials/modules/<module>/config.conf`, and each can be switched off on its
own: a disabled module registers no commands, no listeners and no database migrations, and holds no runtime
state.

Set `enabled = false` in a module's `config.conf` and restart, or edit and run `/uxmess reload <module>`.

| Module | What it does | Default |
|---|---|---|
| [Command Control](commandcontrol.md) | Decide which commands each player may run, and hide the rest | on |
| [Communication](communication.md) | Chat format, join and quit lines, announcer, info pages | on |
| [Custom Commands](customcommands.md) | Your own commands, declared in a file | on |
| [Custom Menus](custommenus.md) | Your own menus, behind `/menu` | on |
| [Discord Link](discordlink.md) | Bind a Minecraft account to a Discord account | off |
| [Economy](economy.md) | Balances, `/pay`, `/baltop`, banks, loans, banknotes | on |
| [Holograms](holograms.md) | Floating displays with pages and click actions | on |
| [Homes](homes.md) | Private teleport points in a slot grid | on |
| [Inventory Rollback](invrollback.md) | Snapshots of an inventory at death and logout | off |
| [Items & World](itemworld.md) | The item, block, entity and environment toolbox | on |
| [Kits](kits.md) | Item bundles claimed on a cooldown | on |
| [Messaging](messaging.md) | Private messages, mail, ignore lists, social spy | on |
| [Moderation](moderation.md) | Bans, mutes, jails, warnings, history | on |
| [Nametags](nametags.md) | The name above each player's head | off |
| [NPCs](npc.md) | Packet-rendered characters with action chains | on |
| [Player State](playerstate.md) | Heal, fly, game mode, speed, personal time and weather | on |
| [Player Warps](playerwarps.md) | Player-owned public destinations | on |
| [Poses](poses.md) | Sit, lay, belly-flop, spin and crawl | on |
| [Presence](presence.md) | AFK, `/list`, nicknames, `/whois`, server health | on |
| [Ranks](ranks.md) | A rankup ladder with prestige and autorank | on |
| [Regions](regions.md) | An in-game manager for WorldGuard regions | off |
| [Scoreboard](scoreboard.md) | The per-player sidebar | off |
| [Security](security.md) | Two-factor, join verification, op protection, alt guard | on |
| [Server Tweaks](servertweaks.md) | F3 brand, console filter, unsigned chat | on |
| [Skin](skin.md) | Skins by name, link or file, with Bedrock and cracked covered | off |
| [Staff](staff.md) | An on-duty mode with a gadget loadout | on |
| [Survival](survival.md) | Tree-feller, veinminer, auto-pickup and the rest | off |
| [Tablist](tablist.md) | Tab-list header, footer, layout and skins | off |
| [Teleport](teleport.md) | `/tpa`, `/back`, `/rtp`, spawns and staff teleports | on |
| [Trade](trade.md) | A shared window for swapping items and money | on |
| [Vanish](vanish.md) | Layered invisibility with see and use levels | on |
| [Vaults](vaults.md) | Personal chest-style storage in the database | on |
| [Villagers](villagers.md) | Trading, restock, protection, pickup and follow | off |
| [Vote](vote.md) | Listing-site votes turned into rewards | off |
| [Warps](warps.md) | Server-wide destinations with costs and locks | on |
| [Worlds](worlds.md) | Create, import, tune and gate worlds | on |
