---
title: Admin commands
order: 203
description: Every /uxmquests branch, its node and what it does.
icon: shield
---

| Command | Node | Does |
|---|---|---|
| `/uxmquests` | `uxmquests.admin.version` | The usage line |
| `/uxmquests version` | `uxmquests.admin.version` | The running version |
| `/uxmquests reload` | `uxmquests.admin.reload` | Reads `config.conf`, the groups, the passes, the windows, the messages and the theme again |
| `/uxmquests doctor` | `uxmquests.admin.doctor` | Checks the storage and the windows |
| `/uxmquests types` | `uxmquests.admin.inspect` | Every task type a group file may write |
| `/uxmquests problems` | `uxmquests.admin.inspect` | What the last load of the files could not read |
| `/uxmquests totals <player>` | `uxmquests.admin.inspect` | How many quests a player has ever finished |
| `/uxmquests pass top <pass> <season>` | `uxmquests.admin.inspect` | The top of a pass; season `0` is all time |
| `/uxmquests trigger <player> <trigger> <amount>` | `uxmquests.admin.trigger` | Advances `trigger` tasks of that name |
| `/uxmquests finish <player> <group> <quest>` | `uxmquests.admin.finish` | Finishes one quest of a player's board; the reward is paid the ordinary way |
| `/uxmquests reset board <player> <group>` | `uxmquests.admin.reset` | Throws a board away, so the next look draws a new one |
| `/uxmquests reset totals <player>` | `uxmquests.admin.reset` | Forgets everything a player has finished |
| `/uxmquests pass xp <player> <amount>` | `uxmquests.admin.pass` | Gives pass experience, with every booster applied |
| `/uxmquests pass booster <player\|server> <multiplier> <time>` | `uxmquests.admin.pass` | Starts a timed booster |
| `/uxmquests lang set-player <player> <language>` | `uxmquests.admin.language` | Sets the language another player reads |
| `/uxmquests lang reset-player <player>` | `uxmquests.admin.language` | Clears another player's choice |

`trigger` is how any plugin that can run a command (a vote listener, a crate, a command block) feeds a quest with no
integration on either side; see [Task types](../features/task-types.md#trigger).

Related: [Permissions](../permissions/reference.md)
