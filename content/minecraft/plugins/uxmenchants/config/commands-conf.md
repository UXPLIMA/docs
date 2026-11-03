---
title: commands.conf
order: 606
description: The name, aliases and switch of every command and subcommand, and words per language.
icon: square-terminal
---

| Key | Ships as | Subcommand keys |
|---|---|---|
| `enchants` | `/enchants`, `/enchantments`, `/ce` | `groups`, `info`, `list`, `limit`, `descriptions`, `lang`, `lang-set`, `lang-reset`, `lang-set-player`, `lang-reset-player` |
| `uxmenchants` | `/uxmenchants`, `/uxme`, `/enchantsadmin` | `version`, `doctor`, `reload`, `editor`, `apply`, `remove`, `give`, `giverandom`, `export`, `strip`, `import` |

A word under a word is keyed with a hyphen and written with a space in its `name`: `lang-set { name = "lang set" }`.

| Key | What it does |
|---|---|
| `name` | The label typed |
| `aliases` | Other labels |
| `enabled` | `false` and the command is never registered |
| `localized-aliases` | Words for the readers of one language: `localized-aliases { tr = ["buyu"] }` |
| `subcommands` | One block per subcommand; `enabled = false` takes that word out |

A change needs a restart: the server builds its command tree once. A renamed subcommand keeps its node. The operator
command of every UXM plugin answers to `<player command>admin` as well, which is why `/enchantsadmin` is an alias.

## Words per language

A word in `localized-aliases` on a command works for anyone who types it, but only a player who reads that language
finds it in their command list. A word under a subcommand is offered to, and works for, the readers of that language
and the console only. The tag is a language (`tr`) or a language and a country (`pt-BR`).

Related: [Commands](../commands/overview.md)
