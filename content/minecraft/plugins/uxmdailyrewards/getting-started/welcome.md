---
title: Welcome
order: 201
description: What uxmDailyRewards does, who it is for, and where to read next.
icon: hand
---

uxmDailyRewards gives each player one reward a day from a calendar. A player who comes back day after day walks the
calendar and builds a streak; a player who misses a day starts again at day one. Every day is its own file, a rank
can get a better reward than everybody else, and a window shows the whole month at a glance.

## What it does

| Part | What it gives you |
|---|---|
| The calendar | 28 days by default, one file each, wrapping back to day one after the last |
| The streak | Days claimed in a row, kept past the end of the calendar; a missed day resets it |
| Rewards | Any action lines per day: commands, money, experience, items, titles, sounds |
| Ranks | A reward per rank on any day, from LuckPerms groups or permission nodes |
| The window | `/dailyrewards` opens the calendar; a click on the ready day claims it |
| The reminder | A title, an action bar line and a sound a few seconds after a player joins with a reward waiting |
| Celebration | Lines that run after every claim, on top of the day's reward |
| Economies | Vault, VaultUnlocked, PlayerPoints, EcoBits, Treasury, experience and levels |
| Storage | SQLite, or MySQL, MariaDB or PostgreSQL shared across a network |
| Placeholders | The streak, the next day, whether it is ready, and the time left |

## Who it is for

Any server that wants players to come back each day: a lobby, a survival server, a network where the streak follows
the player from server to server.

## Where to go next

| You want to | Read |
|---|---|
| Install it | [Installation](installation.md) |
| Understand the window, the grace and the streak | [Concepts](concepts.md) |
| Claim a day and change one | [Your first claim](first-claim.md) |
| Write a day | [Day files](../rewards/day-files.md) |
| Give a rank more | [Ranks](../rewards/ranks.md) |
| Change the window | [Calendar window](../menus/calendar.md) |
