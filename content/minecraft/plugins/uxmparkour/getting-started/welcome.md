---
title: Welcome
order: 201
description: What a run is, and what comes with the plugin.
icon: door-open
---

uxmParkour builds a parkour course in front of a player while they jump. There is no map to build and no
checkpoint to place: an operator marks a box in the sky and a start block, and every run in it is generated from
scratch, one jump at a time. The blocks ahead appear as the player lands, the blocks behind vanish, and the run ends
when they fall.

It runs on Paper 26.2 or newer and on Folia, on Java 25. [Installation](installation.md) has the details.

## What a run is

| Piece | What it means |
|---|---|
| **Arena** | A box in one world, a start block inside it and a heading. Every run in the arena stays inside the box. |
| **Course** | The blocks of one run. A few stand ahead of the player and a few behind; the rest are not in the world. |
| **Difficulty** | The range of jumps the course may use: how far forward, how far sideways, how far up or down. |
| **Theme** | The blocks the course is built from, one drawn at random for every block placed. |
| **Jump** | One landing on the next block. The run's score is its number of jumps. |
| **Fall** | Dropping more than `run.max-fall` blocks below the block last stood on. It ends the run. |
| **Milestone** | A jump count that pays a reward and plays a moment, such as 25 or 100 jumps. |
| **Boards** | The longest run, the most jumps and the most runs, each for all time, the month, the week and the day. |

Each player picks their own theme and difficulty; an arena only sets what a player gets when they have picked
none. [Concepts](concepts.md) explains how the parts fit.

## What comes with it

| Area | What you get |
|---|---|
| Course generator | Only jumps a player can physically make, in any heading, turning inside the box when it runs out of room |
| Difficulties | `easy`, `normal` and `hard`, and any you write ([Difficulties](../arenas/difficulties.md)) |
| Themes | `stone`, `wood`, `quartz`, `glow` and `wool`, and any you write ([Themes](../arenas/themes.md)) |
| Rewards | Milestones that pay experience, levels or money through Vault, VaultUnlocked, PlayerPoints, EcoBits or Treasury ([Rewards](../arenas/rewards.md)) |
| Moments | What a fall, a milestone or a personal best says, shows and sounds like, as lines you write ([Moments](../arenas/moments.md)) |
| Menus | A theme picker for players, and an arena list and editor for operators ([Menus](../menus/index.md)) |
| Leaderboards | Three boards in four periods, for holograms and scoreboards ([Placeholders](../placeholders/reference.md)) |
| Languages | English, Turkish, German, Spanish, French, Portuguese and Russian, per player ([Languages](../config/languages.md)) |
| Database | SQLite out of the box, or MySQL, MariaDB or PostgreSQL shared by a network ([Database](../database/index.md)) |

A player needs one command, `/parkour`. Everything an operator does can be done from a menu or typed, the console
included.

## Where to go next

| You want | Page |
|---|---|
| Get it running | [Installation](installation.md) |
| Understand arenas, courses and boards | [Concepts](concepts.md) |
| Build an arena step by step | [Your first arena](first-arena.md) |
| The full command tree | [Commands](../commands/index.md) |
| Change what a run pays | [Rewards](../arenas/rewards.md) |
| Put a leaderboard on a hologram | [Placeholders](../placeholders/reference.md) |
