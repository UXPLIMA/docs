---
title: Welcome
order: 101
description: What an egg hunt is with uxmEasterEggs, and what comes with the plugin.
icon: door-open
---

uxmEasterEggs turns blocks of your map into eggs for players to find. An operator looks at a block, usually a
player head, and registers it as an egg of one type. A player who clicks it finds it: they are paid, they hear and
see what the type says, and their count goes up. Find enough and a milestone pays; find them all and the server
hears about it.

It runs on Paper 26.2 or newer and on Folia, on Java 25. [Installation](installation.md) has the details.

## What a hunt is

| Piece | What it means |
|---|---|
| **Egg** | One block of the map registered as an egg. The plugin does not place the block; it remembers its position |
| **Type** | What an egg is: its name, its head, its reward, its particle, what finding it does, and whether it can be found again. One file each in `types/` |
| **Find** | A player clicking an egg. The first find counts; a type with a cooldown can be found again for its reward |
| **Milestone** | A number of eggs found that pays a reward, such as 5 or 25 |
| **Completion** | Finding every egg on the map |
| **Radar** | A hint over the hotbar that says how far the nearest unfound egg is |
| **Idle particle** | A particle drawn on an egg for the players who have not found it yet |
| **Board** | The eggs found, for all time, the month, the week and the day |

[Concepts](concepts.md) explains how the parts fit.

## What comes with it

| Area | What you get |
|---|---|
| Types | `common` and `golden`, and any you write, each with its own reward and effects ([Types](../hunting/types.md)) |
| Placing | Look at a block and run one command, or pick a type from a window ([Placing eggs](../hunting/overview.md#placing-an-egg)) |
| Protection | An egg cannot be broken, burnt, blown up, moved by a piston or washed away ([Eggs and finds](../hunting/overview.md#protection)) |
| Rewards | Per type, per milestone and for completion, through experience, levels, Vault, VaultUnlocked, PlayerPoints, EcoBits or Treasury, and console commands ([Rewards](../hunting/rewards.md)) |
| Radar and particles | A distance hint and idle particles, both shared tasks that cost the same however large the map ([Radar and particles](../hunting/radar-and-particles.md)) |
| Moments | What progress, a milestone, completion and the radar say, show and sound like ([Moments](../hunting/moments.md)) |
| Menus | A collection and a leaderboard for players, a list of placed eggs and a type picker for operators ([Menus](../menus/index.md)) |
| Leaderboards | One board in four periods, for holograms and scoreboards ([Placeholders](../placeholders/reference.md)) |
| Languages | English, Turkish, German, Spanish, French, Portuguese and Russian, per player ([Languages](../config/languages.md)) |
| Database | SQLite out of the box, or MySQL, MariaDB or PostgreSQL shared by a network ([Database](../database/index.md)) |

## Where to go next

| You want | Page |
|---|---|
| Get it running | [Installation](installation.md) |
| Understand types, finds and milestones | [Concepts](concepts.md) |
| Hide the first eggs | [Your first hunt](first-hunt.md) |
| Write a type of your own | [Type files](../config/type-files.md) |
| Change what a find pays | [Rewards](../hunting/rewards.md) |
| Put the board on a hologram | [Placeholders](../placeholders/reference.md) |
