---
title: modules.conf
order: 602
description: Which parts of the plugin run, and the capabilities they need.
icon: toggle-right
---

Which parts of the plugin run. A part turned off uses nothing: no listener, no timer, no cache. A change here needs a restart.

The file is `plugins/uxmSkyblock/modules.conf`. Every default below is the shipped value.

## `modules`

| Key | Default | What it does |
|---|---|---|
| `core` | `true` | Islands themselves: creating, joining, homes and the grid. Every other module needs it. |
| `bank` | `true` | The island bank: money and crystals the members share (modules/bank.conf). |
| `upgrades` | `true` | Upgrades an island buys: size, members, generator, warps and more (modules/upgrades.conf). |
| `biomes` | `true` | Changing the biome of an island (modules/biomes.conf). |
| `presets` | `true` | The kinds of island a player can start with (modules/presets.conf). |
| `seasons` | `true` | Seasons that end, pay the best islands and start again (modules/seasons.conf). |
| `social` | `true` | Ratings and the guestbook (modules/social.conf). |
| `discord` | `true` | Messages to Discord webhooks (modules/discord.conf). |
| `alliances` | `true` | Alliances between islands (modules/alliances.conf). |
| `leaderboards` | `true` | The island leaderboards, rebuilt on a schedule and announced when the leader changes. |
| `shop` | `true` | The island shop (modules/shop.conf). |
| `temporary-access` | `true` | /is trust (modules/temporary-access.conf). |
| `rewards` | `true` | The reward inbox (modules/rewards.conf). |
| `warps` | `true` | Island warps (modules/warps.conf). |
| `vault` | `true` | The shared island vault (modules/vault.conf). |
| `chat` | `true` | Island chat (modules/chat.conf). |
| `inactivity` | `true` | What happens to an island whose owner or whole team stops playing (modules/inactivity.conf). |
| `admin-freeze` | `true` | /is admin freeze, which stops an island until staff lift it. |
| `missions` | `true` | Island missions and challenges (modules/missions.conf). |
| `boundary` | `true` | The edge of an island: its border and what may cross it. |
| `recycle` | `true` | Deleting and resetting islands, and giving their place on the grid to new ones. |
| `worth` | `true` | Island worth and level from the blocks it holds (modules/levels.conf). |
| `dimensions` | `true` | The Nether and the End of each island (modules/dimensions.conf). |
| `limits` | `true` | Limits on hoppers, pistons, spawners and creatures (modules/limits.conf). |
| `anti-abuse` | `true` | Reset limits, quarantine and join cooldowns (modules/anti_abuse.conf). |
| `boosters` | `true` | Island boosters (modules/boosters.conf). |
| `bank-upkeep` | `true` | Upkeep an island pays from its bank, and what happens when it cannot (modules/bank.conf). |

## `capabilities`

Which module answers for a job two modules could do. The island bank is the bank module's.

## `capabilities.island-bank`

| Key | Default | What it does |
|---|---|---|
| `selected-provider` | `bank` |  |
