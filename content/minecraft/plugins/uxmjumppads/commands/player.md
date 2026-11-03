---
title: Player commands
order: 202
description: The /jumppad language subcommands.
icon: user
---

`/jumppad`, alias `/jumppads`, carries only the language a player reads this plugin in. Using a pad needs no command.

| Command | Permission | What it does |
|---|---|---|
| `/jumppad lang` | `uxmjumppads.language` | Says which language you read uxmJumpPads in, and which the server has. Player only |
| `/jumppad lang set <language>` | `uxmjumppads.language` | Reads uxmJumpPads in that language from now on. Player only |
| `/jumppad lang reset` | `uxmjumppads.language` | Forgets your choice; you read your client's language again. Player only |
| `/jumppad lang set-player <player> <language>` | `uxmjumppads.admin.language` | Sets another player's language |
| `/jumppad lang reset-player <player>` | `uxmjumppads.admin.language` | Clears another player's choice |

The plugin's own lines are few, the replies to operators and what pad effects say with an `@key`, so most players
never need this. With uxmLang installed, the choice is uxmLang's. See [Languages](../config/languages.md).

Related: [Overview](overview.md), [Operator commands](admin.md)
