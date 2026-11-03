---
title: commands.conf
order: 504
description: The name, aliases and switch of every command and subcommand, and words per language.
icon: square-terminal
---

| Key | Ships as | Subcommand keys |
|---|---|---|
| `auction` | `/auction`, `/ah` | `mailbox`, `mine`, `bids`, `orders`, `history`, `watch`, `search`, `profile`, `sell`, `auction`, `reprice`, `buy`, `bid`, `order`, `fill`, `cancel`, `claim`, `lang`, `"lang set"`, `"lang reset"`, `"lang set-player"`, `"lang reset-player"` |
| `uxmauction` | `/uxmauction`, `/auctionadmin`, `/ahadmin` | `cancel`, `return`, `purge`, `log`, `restore`, `stats`, `lookup`, `lock`, `doctor`, `reload`, `version` |

| Key | What it does |
|---|---|
| `name` | The label typed |
| `aliases` | Other labels |
| `enabled` | `false` and the command is never registered |
| `localized-aliases` | Words for the readers of one language |
| `subcommands` | One block per subcommand; `enabled = false` takes that word out |

A change needs a restart. A renamed subcommand keeps its node.

## Words per language

A word in `localized-aliases` on a command works for anyone who types it, but only a player who reads that language
finds it in their command list. A word under a subcommand is offered to, and works for, the readers of that language
and the console only.

Related: [Commands](../commands/overview.md)
