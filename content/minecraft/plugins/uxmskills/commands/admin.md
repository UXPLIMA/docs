---
title: Admin commands
order: 203
description: Every /uxmskills branch, its node and what it does.
icon: shield
---

| Command | Node | Does |
|---|---|---|
| `/uxmskills` | `uxmskills.admin.version` | The usage line |
| `/uxmskills version` | `uxmskills.admin.version` | Which build this is |
| `/uxmskills reload` | `uxmskills.admin.reload` | Reads every file again |
| `/uxmskills doctor` | `uxmskills.admin.doctor` | Storage, skills, currencies, placeholders, claims, windows |
| `/uxmskills curves` | `uxmskills.admin.reload` | Prints what each curve preset costs |
| `/uxmskills recalculate` | `uxmskills.admin.reload` | Rewrites every stored level after a curve change |
| `/uxmskills give <player> <skill> <amount>` | `uxmskills.admin.experience` | Gives experience |
| `/uxmskills take <player> <skill> <amount>` | `uxmskills.admin.experience` | Takes experience |
| `/uxmskills set <player> <skill> <amount>` | `uxmskills.admin.experience` | Sets experience |
| `/uxmskills setlevel <player> <skill> <level>` | `uxmskills.admin.experience` | Puts a player at a level |
| `/uxmskills addlevel <player> <skill> <levels>` | `uxmskills.admin.experience` | Moves a player by levels |
| `/uxmskills reset <player> <skill>` | `uxmskills.admin.reset` | Clears one skill |
| `/uxmskills resetall <player>` | `uxmskills.admin.reset` | Clears every skill of a player |
| `/uxmskills wipe <confirm>` | `uxmskills.admin.wipe` | Clears every skill of every player |
| `/uxmskills boost <player> <multiplier> <minutes>` | `uxmskills.admin.boost` | Boosts one player |
| `/uxmskills boostall <multiplier> <minutes>` | `uxmskills.admin.boost` | Boosts the whole server |
| `/uxmskills boostoff` | `uxmskills.admin.boost` | Takes every boost away |
| `/uxmskills markitem <multiplier>` | `uxmskills.admin.boost` | Marks the held item as a multiplier; players only |
| `/uxmskills markitem skill <skill> <multiplier>` | `uxmskills.admin.boost` | The same, for one skill; players only |
| `/uxmskills markitem stat <stat> <amount>` | `uxmskills.admin.boost` | Marks it as a stat modifier; players only |
| `/uxmskills statgive <player> <stat> <name> <amount> <minutes>` | `uxmskills.admin.stats` | A named, timed stat modifier |
| `/uxmskills stattake <player> <stat> <name>` | `uxmskills.admin.stats` | Takes one off |
| `/uxmskills backup` | `uxmskills.admin.backup` | Writes a backup now |
| `/uxmskills restore <file>` | `uxmskills.admin.backup` | Reads a backup from `backups/` |
| `/uxmskills inspect <player>` | `uxmskills.admin.inspect` | What a player holds |
| `/uxmskills diagnose` | `uxmskills.admin.diagnose` | Writes a report to `reports/` |
| `/uxmskills import <folder>` | `uxmskills.admin.import` | Reads AuraSkills player files |
| `/uxmskills lang set-player <player> <language>` | `uxmskills.admin.language` | Another player's language |
| `/uxmskills lang reset-player <player>` | `uxmskills.admin.language` | Clears another player's choice |

Related: [Data and backups](../features/data.md)
