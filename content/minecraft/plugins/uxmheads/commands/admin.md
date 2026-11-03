---
title: Operator commands
order: 202
description: Every /uxmheads subcommand, its permission and what it answers.
icon: shield
---

`/uxmheads`, aliases `/heads` and `/uxmh`.

| Command | Permission | What it does |
|---|---|---|
| `/uxmheads` | `uxmheads.admin.version` | Says to add `help` |
| `/uxmheads preview <player>` | `uxmheads.admin.preview` | Prints a player's face as you see it in chat. Works for a player who is away, through `find-by-name` |
| `/uxmheads refresh <player>` | `uxmheads.admin.refresh` | Reads an online player's skin again, after they changed it |
| `/uxmheads welcome <player>` | `uxmheads.admin.welcome` | Sends an online player the welcome again |
| `/uxmheads reload` | `uxmheads.admin.reload` | Reads the configuration and the messages again |
| `/uxmheads version` | `uxmheads.admin.version` | The running version |
| `/uxmheads doctor` | `uxmheads.admin.doctor` | The health checks, one line each |
| `/uxmheads lang` | `uxmheads.language` | The language you read, and the choices |
| `/uxmheads lang set <language>` | `uxmheads.language` | Chooses a language |
| `/uxmheads lang reset` | `uxmheads.language` | Follows the client again |
| `/uxmheads lang set-player <player> <language>` | `uxmheads.admin.language` | Sets another player's language; one who is away gets it at their next join |
| `/uxmheads lang reset-player <player>` | `uxmheads.admin.language` | Clears another player's choice |

`refresh` and `welcome` answer `error.not-online` for a player who is not on the server.

A reload is swapped in whole; a file that does not parse answers `admin.reload-failed` and the server keeps what
worked.

Related: [Overview](overview.md), [Permissions](../permissions/reference.md)
