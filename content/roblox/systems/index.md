---
title: Systems
order: 2015
description: Drop-in Luau systems for Roblox experiences, from daily rewards to a full vehicle pack.
icon: blocks
---

Every UXR system is a single folder you drop into `ServerScriptService`. It installs
itself on the first server start: no manual wiring, no scripts to place, no services to
register. Configuration is a small number of commented Luau files.

Start with [How a UXR system works](platform/) if this is your first one. It covers the
parts every system shares, so each product's own pages can stay on what makes it different.

## Progression and rewards

| System | What it does |
|---|---|
| [uxrDailyRewards](uxrdailyrewards/) | A daily login ladder with streaks, a skip-the-wait product, and open-ended reward code |
| [uxrLevel](uxrlevel/) | XP and levels with a rank ladder, tool unlocks and an overhead bar |

## Games

| System | What it does |
|---|---|
| [uxrArcadeMachine](uxrarcademachine/) | A playable arcade cabinet with pluggable mini games |
| [uxrBilliards](uxrbilliards/) | Physics pool with cue aiming, shot power and full rules |
| [uxrBowling](uxrbowling/) | Ten-pin bowling lanes with scoring and frames |
| [uxrChess](uxrchess/) | Full-rules chess with a built-in engine opponent |
| [uxrColorCards](uxrcolorcards/) | A shedding card game for two to eight players |
| [uxrDarts](uxrdarts/) | Dartboards with scoring modes and throw physics |
| [uxrTicTacToe](uxrtictactoe/) | Tic tac toe with a perfect-play bot |

## Roleplay and world

| System | What it does |
|---|---|
| [uxrVehicleSystemPack](uxrvehiclesystempack/) | Drivable vehicles with seats, ownership and a spawn menu |
| [uxrDoor](uxrdoor/) | Doors and gates with permissions, keys and animation |
| [uxrHandcuff](uxrhandcuff/) | Cuffing, dragging and escaping, for police roleplay |
| [uxrPet](uxrpet/) | Followers with eggs, rarity, hatching and equipping |
| [uxrTroll](uxrtroll/) | Paid troll actions players buy and aim at each other |

## Presentation and staff

| System | What it does |
|---|---|
| [uxrOverhead](uxroverhead/) | Nameplates above every player, driven by your own data |
| [uxrAdminPanel](uxradminpanel/) | An in-game admin panel with ranks, commands and logging |
