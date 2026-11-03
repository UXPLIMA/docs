---
title: Welcome
order: 201
description: What a round of King of the Ladder is, and what comes with the plugin.
icon: door-open
---

uxmKotl is King of the Ladder for a lobby or a minigame server. A tower stands inside a box, a plate sits on top
of it, and the player who stands on the plate wears the crown. Everyone else in the box holds a knockback stick and
tries to push the king off. The longer a player holds the plate, the higher they climb the boards.

It runs on Paper 26.2 or newer and on Folia, on Java 25. [Installation](installation.md) has the details.

## What a round is

| Piece | What it means |
|---|---|
| **Arena** | A box in one world with a plate inside it, a spawn and an exit. One file per arena under `arenas/`. |
| **Boundary** | The box. A player is in the arena while they stand inside it; stepping out leaves. |
| **Plate** | One block inside the box. The player whose feet are in that block holds the crown. |
| **Crown** | The title of king. One per arena, held by one player at a time or by nobody. |
| **Reign** | One spell as king, from the claim until the king steps off, dies, leaves or loses the plate. |
| **Claim cooldown** | A new king is protected for 1.5 seconds, so two players on one block do not trade the crown on every step. |
| **Knockback stick** | The weapon every player is given on entry, with Knockback II by default. It cannot be dropped or moved. |
| **Kill** | A player in the arena who dies to another player in the same arena. The killer gets the kill. |
| **Boards** | Time held, longest reign, crowns taken and kills, each for all time, the month, the week and the day. |

There is no round timer and no lobby queue: the arena is always open, a player walks in and plays, and walks out
when they are done. [Concepts](concepts.md) explains how the parts fit.

## What comes with it

| Area | What you get |
|---|---|
| Arenas | Any number, each in its own file, built by command or in a menu ([Arenas](../arenas/overview.md)) |
| Joining | By walking into the box, or by `/kotl join <arena>`, per arena ([Joining and leaving](../arenas/overview.md#joining-and-leaving)) |
| The crown | A ring of particles over the king's head, a claim cooldown, an optional push off a protected plate ([The crown](../arenas/crown.md)) |
| Fights | A knockback stick in a fixed hotbar slot, deaths that keep everything, kills credited to players in the same arena ([Fights](../arenas/fights.md)) |
| Rewards | Money or commands for every crown taken and every kill, through experience, levels, Vault, VaultUnlocked, PlayerPoints, EcoBits or Treasury ([Rewards](../arenas/rewards.md)) |
| Moments | What a claim, a lost crown, a kill or a join says, shows and sounds like, as lines you write ([Moments](../arenas/moments.md)) |
| Menus | A record window and a leaderboard window for players, an arena list and editor for operators ([Menus](../menus/index.md)) |
| Leaderboards | Four boards in four periods, for holograms and scoreboards ([Placeholders](../placeholders/reference.md)) |
| Languages | English, Turkish, German, Spanish, French, Portuguese and Russian, per player ([Languages](../config/languages.md)) |
| Database | SQLite out of the box, or MySQL, MariaDB or PostgreSQL shared by a network ([Database](../database/index.md)) |

A player needs no command at all on an arena that joins by walking in. Everything an operator does can be done from
a menu or typed, the console included.

## Where to go next

| You want | Page |
|---|---|
| Get it running | [Installation](installation.md) |
| Understand arenas, reigns and boards | [Concepts](concepts.md) |
| Build an arena step by step | [Your first arena](first-arena.md) |
| The full command tree | [Commands](../commands/index.md) |
| Pay players for the crown | [Rewards](../arenas/rewards.md) |
| Put a leaderboard on a hologram | [Placeholders](../placeholders/reference.md) |
