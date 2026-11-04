---
title: Overview
order: 301
description: The roots, their aliases, and renaming any word in commands.conf.
icon: list
---

| Root | Aliases | Turkish word | For |
|---|---|---|---|
| `/bedrock` | `/be` | `/bedrockayar` | A Bedrock player's own settings |
| `/menus` | | `/menüler` | Forms or chests for other plugins' menus |
| `/uxmbedrock` | `/bedrockadmin` | | Operators |

Every command and every word under it is named in [commands.conf](../config/commands-conf.md), so you can rename a
word, add an alias or turn a command off. A change there needs a restart.

The player commands answer only Bedrock players; a Java player who types one is told it is for Bedrock Edition. The
operator command works from the console too, except `gui measure`, which needs a player who opens a menu.
