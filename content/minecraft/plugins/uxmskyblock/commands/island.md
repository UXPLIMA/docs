---
title: Your island
order: 202
description: Starting, travelling, homes, the border, biome, level, reset and the language you read.
icon: house
---

| Command | What it does | Permission |
|---|---|---|
| `/is` , `/is menu` | Opens the island menu, or the start window when you have no island | |
| `/is create [preset]` | Starts your island from a preset; with no preset, opens the start window | `uxmskyblock.island.create` |
| `/is home` | Takes you to your island's spawn | `uxmskyblock.island.home` |
| `/is nether` | Takes you to your island's Nether | |
| `/is end` | Takes you to your island's End | |
| `/is setspawn` | Makes where you stand the spawn every member and visitor arrives at | |
| `/is sethome <name>` | Saves where you stand as a named home | `uxmskyblock.island.sethome` |
| `/is gohome <name>` | Takes you to a named home | `uxmskyblock.island.home` |
| `/is homes` | Opens the [homes window](../menus/travel.md#homes) | |
| `/is homes list` | Opens your homes one tile each, or lists them in chat when `menus/island-home-list.conf` is removed | |
| `/is delhome <name>` | Deletes a named home | |
| `/is rename <name>` | Renames your island; the name shows on the leaderboards | |
| `/is info` | Shows the owner, members, state and visitor access of your island, and its boosters | |
| `/is border` | Opens the [border window](../menus/island.md#border): show, hide and colour the border | |
| `/is biome [list \| <type>]` | Opens the biomes, lists them, or changes the island biome | `uxmskyblock.island.biome` |
| `/is level [recalculate]` | Shows the island level and worth, or counts the blocks again | |
| `/is worth` | Shows what the island is worth | |
| `/is values` | Opens the [block values](../menus/economy.md#block-values): what each block adds | |
| `/is limits` | Shows how many of each limited block and creature the island holds | |
| `/is quarantine` | Shows whether the island is quarantined, and why | |
| `/is fly` | Flies on an island you belong to, or comes down | `uxmskyblock.island.fly` |
| `/is reset [confirm]` | Resets your island; without `confirm` it opens the [reset window](../menus/island.md#reset) and asks first | |
| `/is delete [confirm]` | Erases your island, the same as reset | `uxmskyblock.island.delete` |
| `/is disband [confirm]` | Gives up your island, the same as reset | `uxmskyblock.island.delete` |
| `/is profile switch <id>` | Switches your active profile | |
| `/is lang [language \| reset]` | Shows or chooses the language you read, for every UXM plugin on the server | |
| `/is help [page]` | Lists the commands you may use | |

Only the owner resets, deletes or disbands an island. How often an island may be reset
is in [anti_abuse.conf](../config/anti_abuse-conf.md): 3 resets a day and a wait between two, as shipped. What a reset takes from the members is in [lifecycle.conf](../config/lifecycle-conf.md).
