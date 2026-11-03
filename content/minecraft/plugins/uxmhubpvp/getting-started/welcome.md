---
title: Welcome
order: 201
description: What lobby PvP is with uxmHubPvP, and what comes with the plugin.
icon: door-open
---

uxmHubPvP lets the players in a lobby fight each other, but only the ones who ask to. A player clicks the sword in
their hotbar or types `/pvp`, stands still for three seconds, and gets a kit; their own items wait in the database
until they leave. Everybody else in the lobby cannot hit them and cannot be hit by them. There is no arena to build:
the fight happens wherever the fighters are.

It runs on Paper 26.2 or newer and on Folia, on Java 25. [Installation](installation.md) has the details.

## What a fight is

| Piece | What it means |
|---|---|
| **PvP on** | A player who has turned PvP on. Only two such players can hurt each other |
| **Toggle item** | The sword in the hotbar that turns PvP on, and the red dye that turns it off again |
| **Warm-up** | Standing still before the switch happens: 3 seconds in, 8 seconds out. Moving cancels it |
| **Kit** | The armour, weapon, shield and food a fighter is handed. Their own inventory is kept safe meanwhile |
| **Combat tag** | 10 seconds after a hit in which a fighter can type only a few commands, and quitting counts as a death |
| **Kill** | A fighter killed by another player. The killer's kills and streak go up, and the kill pays |
| **Streak** | Kills in a row without dying. The best streak is kept |
| **Boards** | Kills and the best streak, each for all time, the month, the week and the day |

[Concepts](concepts.md) explains how the parts fit.

## What comes with it

| Area | What you get |
|---|---|
| Opting in | A toggle item and `/pvp`, with warm-ups that stop a losing player from escaping at once ([Turning PvP on and off](../fighting/overview.md)) |
| A safe inventory | The player's own items are written to the database before the kit is given, and come back after a crash ([The kit](../fighting/kit.md)) |
| Combat tags | A command whitelist for tagged players, and a death for quitting mid-fight ([Combat tag](../fighting/combat-tag.md)) |
| Deaths and kills | No drops, an instant respawn, kills, deaths, ratio and streaks ([Deaths and kills](../fighting/deaths-and-kills.md)) |
| Rewards | Every kill pays, through experience, levels, Vault, VaultUnlocked, PlayerPoints, EcoBits or Treasury ([Rewards](../fighting/rewards.md)) |
| Moments | What turning on, a warm-up, a tag, a kill and a death say, show and sound like ([Moments](../fighting/moments.md)) |
| Other games | Steps aside inside another plugin's game, such as a round of uxmKotl ([Other games](../integrations/other-games.md)) |
| Leaderboards | Two boards in four periods, for holograms and scoreboards ([Placeholders](../placeholders/reference.md)) |
| Languages | English, Turkish, German, Spanish, French, Portuguese and Russian, per player ([Languages](../config/languages.md)) |
| Database | SQLite out of the box, or MySQL, MariaDB or PostgreSQL shared by a network ([Database](../database/index.md)) |

## Where to go next

| You want | Page |
|---|---|
| Get it running | [Installation](installation.md) |
| Understand warm-ups, tags and streaks | [Concepts](concepts.md) |
| Fit it into your lobby | [Setting up the fight](first-fight.md) |
| Change the kit | [kit.conf](../config/kit-conf.md) |
| Pay players for kills | [Rewards](../fighting/rewards.md) |
| Put the boards on a hologram | [Placeholders](../placeholders/reference.md) |
