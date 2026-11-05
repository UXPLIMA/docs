---
title: Configuration
order: 60
description: Every file the plugin writes, reloading, and what an update does to your files.
icon: settings
---

Every file is HOCON: `key = value`, blocks in braces, `#` comments. Each file explains itself in its comments; these
pages list every key with its shipped default.

## Reloading and updating

`/is reload` reads the theme, the language files, the menus and the OneBlock phases again. Everything else is read at
start: change it and restart.

Each start adds to every file the keys a new release brought, and changes no value you wrote. The file as it was is
kept beside it as `<file>.bak`, and the last five copies under `backups/`. A key you deleted stays deleted. To turn
something off, use its switch rather than deleting it.

## The files

| Page | File |
|---|---|
| [config.conf](config-conf.md) | This server, the grid, language, storage, player state, notifications, roles, homes, the REST API, backups, the leaderboard, menu sounds |
| [modules.conf](modules-conf.md) | Which parts of the plugin run |
| [commands.conf](commands-conf.md) | The words of `/island` and `/isadmin` |
| [text-input.conf](text-input-conf.md) | How a menu asks a player to write something |
| [theme.conf](theme.md) | Colours, glyphs and small capitals |
| [Messages](messages.md) | `messages/messages_<language>.conf` |
| [Languages](languages.md) | The seven languages and how a player's language is chosen |
| [Menu files](menu-files.md) | `menus/*.conf` |
| [OneBlock phases](oneblock-phases.md) | `oneblock/phases/*.conf` |
| [Loot tables](loot-tables.md) | `loot/*.conf` |

## Module files

| Page | What it sets |
|---|---|
| [acidisland.conf](acidisland-conf.md) | AcidIsland: the acid sea, acid rain, clean water and the vent |
| [alliances.conf](alliances-conf.md) | Allied islands: visits, chat and how many allies an island keeps |
| [anti_abuse.conf](anti_abuse-conf.md) | Starter item farming: new islands and second accounts |
| [bank.conf](bank-conf.md) | The island bank: currencies, interest, refunds and upkeep |
| [biomes.conf](biomes-conf.md) | Which biomes an island may take, and what it must reach first |
| [boosters.conf](boosters-conf.md) | Timed boosts to spawners, crops, the generator, drops and experience |
| [boxed.conf](boxed-conf.md) | Boxed: the box, its growth, its side and what growth costs |
| [brix.conf](brix-conf.md) | Brix: creative plots, the plot sky and build contests |
| [caveblock.conf](caveblock-conf.md) | CaveBlock: the rock, the room and its tunnels |
| [chat.conf](chat-conf.md) | Island chat, ally chat and who may listen in |
| [chunkblock.conf](chunkblock-conf.md) | ChunkBlock: the magic block, the unlock ladder and chunk prices |
| [dimensions.conf](dimensions-conf.md) | The Nether and the End of an island: own, shared or none |
| [discord.conf](discord-conf.md) | Discord webhooks for milestones, boards, alliances and staff actions |
| [effects.conf](effects-conf.md) | What every interaction fires: sounds, particles, titles and lines |
| [fly.conf](fly-conf.md) | Flight on a player's own island |
| [generators.conf](generators-conf.md) | What the cobblestone generator makes at each tier |
| [inactivity.conf](inactivity-conf.md) | Archiving and deleting islands whose team stopped playing |
| [interactables.conf](interactables-conf.md) | Which blocks count as doors, switches, containers and workstations |
| [island-effects.conf](island-effects-conf.md) | The potion effects upgrades put on players on the island |
| [levels.conf](levels-conf.md) | Island worth and level: block values and the level curve |
| [lifecycle.conf](lifecycle-conf.md) | Resets, deletion and recycling of islands |
| [limits.conf](limits-conf.md) | The most of each costly block and creature one island holds |
| [missions.conf](missions-conf.md) | Island missions: daily, weekly and challenges, and their pay |
| [oneblock.conf](oneblock-conf.md) | OneBlock: the block, its phases, the progress bar and branches |
| [parkour.conf](parkour-conf.md) | Parkour: courses, runs, checks, ghosts, ratings and seasons |
| [performance.conf](performance-conf.md) | How fast islands are cleared and put back |
| [player-state.conf](player-state-conf.md) | What a player carries in each kind of world |
| [poseidon.conf](poseidon-conf.md) | Poseidon: the sea floor island, breath, lore and the deep boss |
| [presets.conf](presets-conf.md) | The islands a player may start, and what each starts with |
| [protection.conf](protection-conf.md) | Rules that keep players from losing what they cannot get back |
| [rewards.conf](rewards-conf.md) | The reward inbox for rewards a player could not be handed |
| [seasons.conf](seasons-conf.md) | Seasons: length, boards and season rewards |
| [settings.conf](settings-conf.md) | The island edge and islands with no member online |
| [shop.conf](shop-conf.md) | The island shop: items, prices and how prices move |
| [skygrid.conf](skygrid-conf.md) | SkyGrid: the grid of lone blocks and its chests |
| [social.conf](social-conf.md) | Ratings and the guestbook |
| [stacking.conf](stacking-conf.md) | Stacked blocks and how they count |
| [strangerrealms.conf](strangerrealms-conf.md) | StrangerRealms: real land with an Upside Down |
| [temporary-access.conf](temporary-access-conf.md) | Trust: letting a visitor build for a while |
| [trade.conf](trade-conf.md) | Trading between players in one window |
| [tradewinds.conf](tradewinds-conf.md) | TradeWinds: the vessel, ports, markets, contracts and piracy |
| [upgrades.conf](upgrades-conf.md) | Island upgrades: every upgrade, its tiers and their prices |
| [vault.conf](vault-conf.md) | The shared island vault and its pages |
| [visitors.conf](visitors-conf.md) | What visitors may do, and the visit fee |
| [walls.conf](walls-conf.md) | The walls between islands |
| [warps.conf](warps-conf.md) | Island warps: how many, categories and who may open them |
| [webmap.conf](webmap-conf.md) | Islands drawn on a web map |
| [world.conf](world-conf.md) | The island world: spawn height, the spawn and starter structures |
