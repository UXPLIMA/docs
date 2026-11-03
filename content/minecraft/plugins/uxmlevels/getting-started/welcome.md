---
title: Welcome
order: 201
description: What uxmLevels does, who it is for, and where to read next.
icon: hand
---

uxmLevels gives players levels for playing. A server writes its own **tracks**, ladders of levels such as global,
mining or combat, and its own **events**, the things that pay experience into a track: playing, mining, fighting,
fishing, eating, crafting and chatting. A track can carry rewards at chosen levels, a player who reaches the top can
prestige to start again with a permanent multiplier, and every track has boards by all time, month, week and day.

## What it does

| Part | What it gives you |
|---|---|
| Tracks | Any number of ladders, one file each, with their own length and cost curve |
| Events | Seven kinds of activity, filtered by block, mob, item or food, with amounts, chances and cooldowns |
| Rewards | Action lines at a level, claimed in a window or given the moment the level is reached |
| Prestige | Reset a finished track for a permanent multiplier and a badge |
| Multipliers | A global one, one per permission, a timed boost per player, and the prestige one |
| Heads-up | The experience bar, a boss bar and the action bar, each showing one track |
| Boards | All time, monthly, weekly and daily boards per track, in chat, a window and placeholders |
| Anti-abuse | Player-placed blocks pay nothing, idle players earn no playtime, an hourly cap |
| Windows | Your tracks, a track's rewards, prestige, and the top players |
| Storage | SQLite, or MySQL, MariaDB or PostgreSQL shared across a network |

## Who it is for

Survival and network servers that want a sense of progress outside vanilla experience, and lobbies that want a
playtime level. The shipped files are an example to start from: three tracks and ten events.

## Where to go next

| You want to | Read |
|---|---|
| Install it | [Installation](installation.md) |
| Understand tracks, events and multipliers | [Concepts](concepts.md) |
| Write a track and an event | [Your first track](first-track.md) |
| See every event type | [Events](../tracks/events.md) |
| Give rewards at a level | [Rewards](../tracks/rewards.md) |
| Put a level on a scoreboard | [Placeholders](../placeholders/reference.md) |
