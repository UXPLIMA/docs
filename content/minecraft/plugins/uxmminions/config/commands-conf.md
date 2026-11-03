---
title: commands.conf
order: 607
description: The name, aliases and switch of every command and subcommand, and words per language.
icon: square-terminal
---

| Key | Ships as | Subcommand keys |
|---|---|---|
| `minions` | `/minions`, `/minion`, `/mn` | `list` (alias `l`), `report`, `trust`, `untrust`, `name`, `consume`, `lang`, `lang-set`, `lang-reset`, `lang-set-player`, `lang-reset-player` |
| `uxmminions` | `/uxmminions`, `/minionsadmin` | `give`, `find`, `open`, `minions`, `remove`, `restore`, `slots`, `ledger`, `migrate`, `editor`, `reload`, `doctor`, `version` |

| Key | What it does |
|---|---|
| `name` | The label typed |
| `aliases` | Other labels |
| `enabled` | `false` and the command or subcommand is never registered |
| `localized-aliases` | Words for the readers of one language |
| `subcommands` | One block per subcommand |

A change needs a restart. A renamed subcommand keeps its node.

## Words per language

A word in `localized-aliases` on a command works for anyone who types it, but only a player who reads that language
finds it in their command list. A word under a subcommand is offered to, and works for, the readers of that language
and the console only.

Related: [Commands](../commands/overview.md)
