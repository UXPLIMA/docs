---
title: Player commands
order: 202
description: Every /quests branch, its node and what it does.
icon: user
---

| Command | Node | Does |
|---|---|---|
| `/quests` | `uxmquests.use` | Opens the [quests window](../menus/quests.md), or writes the boards in chat when the window file is gone |
| `/quests start <group> <quest>` | `uxmquests.use` | Starts a quest a catalogue offers |
| `/quests claim <group> <quest>` | `uxmquests.use` | Takes the reward of a finished quest, in a `manual` group |
| `/quests reroll <group> <quest>` | `uxmquests.reroll` | Swaps a quest for another from the same pool, within the rerolls you have |
| `/quests pass` | `uxmquests.use` | Every pass, the tier you reached, your experience and your track, in chat |
| `/quests pass claim <pass>` | `uxmquests.use` | Takes every pass reward reached and not yet taken, all tiers at once |
| `/quests lang` | `uxmquests.language` | The language you read, and what else the server has |
| `/quests lang set <language>` | `uxmquests.language` | Reads this plugin in one language, whatever the client says |
| `/quests lang reset` | `uxmquests.language` | Follows the client again |

Every branch is for players only; the console is told so.

Related: [Quests window](../menus/quests.md), [Battle pass](../features/battle-pass.md)
