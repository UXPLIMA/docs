---
title: Player commands
order: 302
description: Opening the window, joining a lobby by name, the quick lobby, and the language branch.
icon: user
---

| Command | Node | What it does |
|---|---|---|
| `/lobby` | `uxmmultilobby.use` | Opens the [lobby window](../menus/lobbies.md) |
| `/lobby <id>` | `uxmmultilobby.use` | Joins that lobby; the id is matched without case and completed from the list |
| `/quicklobby` | `uxmmultilobby.use` | Joins the least loaded lobby you may enter; see [Load balancing](../features/balancing.md) |
| `/lobby lang` | `uxmmultilobby.language` | Says which language you read and which ones exist |
| `/lobby lang set <language>` | `uxmmultilobby.language` | Reads this plugin in that language |
| `/lobby lang reset` | `uxmmultilobby.language` | Goes back to the server's choice |

Players only. Every move ends in an [outcome](../features/outcomes.md): `/lobby lobby-9` with no such lobby answers
`no-such-lobby`, a move within the cooldown answers `cooldown`.

With uxmLang installed, the language branch reads and writes the one choice every UXM plugin shares.

Related: [Moving](../features/moving.md)
