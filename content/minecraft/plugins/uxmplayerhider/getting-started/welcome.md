---
title: Welcome
order: 101
description: What uxmPlayerHider does, who it is for, and where to read next.
icon: hand
---

uxmPlayerHider lets each player choose how much of the crowd they see: **everybody**, **only the important
players**, or **nobody**. A dye in the hotbar walks the three states with a right click, and `/hide` does the same
for a player who lost it. The choice is the player's alone, is remembered when they come back, and follows them
across every server that shares the database.

## What it does

| Part | What it gives you |
|---|---|
| Three states | `all`, `important` and `none`; the middle one keeps staff and whoever you mark visible |
| The tool | A dye in the hotbar that changes colour with the state, locked in its slot, in each player's language |
| The command | `/hide` to cycle, `/hide state <state>` to pick one, `/hide status` to count who is hidden |
| Worlds | A list of the worlds it works in; outside them everybody is visible again and the choice waits |
| Cooldown | A gap between two changes, so nobody makes the server work by holding a button |
| Effects | Action lists per state: a title, an action bar, a sound, by default |
| Storage | One row per player who hides anything, on SQLite or a shared MySQL, MariaDB or PostgreSQL |
| Placeholders | The state, whether a player hides anyone, and how many they hide |

## Who it is for

A lobby or hub, where a crowd at spawn makes a slow computer slower and a busy screen busier. Hiding is one way:
the player who hides still sees what they choose, and everybody else still sees them.

## Where to go next

| You want to | Read |
|---|---|
| Install it | [Installation](installation.md) |
| Understand the states and who is important | [Concepts](concepts.md) |
| Try it in two minutes | [Your first hide](first-hide.md) |
| Limit it to the lobby worlds | [Worlds and cooldown](../hiding/worlds-and-cooldown.md) |
| Change what a change of state says | [Effects](../hiding/effects.md) |
| Share the choice across a network | [Database](../database/overview.md) |
