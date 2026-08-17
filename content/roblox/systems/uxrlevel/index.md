---
title: uxrLevel
order: 11
description: Per-team XP and rank ladders with tool rewards, an admin panel and a scripting API.
icon: trending-up
---

XP that accrues while a player is in the game, a named rank ladder per team, tools handed
out automatically at each rank, and an in-game panel for staff to correct someone's XP.

The distinctive part is that levels are **per team**. A player who is a Lieutenant on
Police and a Recruit on Soldier holds both, separately, in one profile. Switching team
switches which ladder is showing without touching either number.

## Pages

- [Setup](setup.md)
- [Levels and teams](levels.md)
- [Tool rewards](tools.md)
- [Settings](settings.md)
- [The admin panel](admin.md)
- [The scripting API](api.md)
- [The interface](interface.md)

## What it does

| | |
|---|---|
| XP source | A timed grant every `XPTime` seconds, plus anything your code adds |
| Ladders | One list of named levels per Roblox team, unlimited length |
| Rank display | Level name, colour, current XP, XP to the next rank, progress percentage |
| Rewards | Tools cloned into the backpack, resynced on every rank change |
| Staff tools | A chat-opened panel to add, remove or set another player's XP |
| Permissions | By team, by user id, or by group rank |
| Saving | One `DataStore` profile per player, holding every team's XP and total playtime |
| Scripting | A server API of roughly forty functions plus three signals |

## Where the numbers live

| File | Holds |
|---|---|
| `Shared/Config/Levels.luau` | The ladders, XP timing, notification toggles, the player chat command |
| `Shared/Config/Admin.luau` | Who is staff, and the command that opens their panel |
| `Shared/Config/Settings.luau` | Saving, Studio behaviour, debug output |

Almost all of your editing is in `Levels.luau`.
