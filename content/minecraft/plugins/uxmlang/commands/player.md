---
title: Player commands
order: 302
description: /lang and its subcommands, with what each answers.
icon: user
---

Node `uxmlang.use`, given to everyone. Players only.

| Command | What it does |
|---|---|
| `/lang` | Opens the [language window](../menus/language.md); without the window file, says the two lines of `/lang list` |
| `/lang list` | The language you read now, and the ones the server has |
| `/lang set <language>` | Reads the server in that language, whatever your client is set to: `tr`, `de`, `pt-BR` |
| `/lang reset` | Follows your client again, as a player who never chose anything does |

| Answer | When |
|---|---|
| `language.changed` | The choice is made; the line is already in the new language |
| `language.already` | You asked for the language you read |
| `language.reset` | You follow your client again |
| `language.unknown` | The server has no such language; the line lists the ones it has |
| `language.forced` | `language.force` is set, so nobody chooses |

What a choice does besides the line is the `effects` block of [config.conf](../config/config-conf.md#effects).

Related: [Choosing a language](../features/choosing.md)
