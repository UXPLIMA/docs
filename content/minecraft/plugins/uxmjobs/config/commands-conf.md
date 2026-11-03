---
title: commands.conf
order: 611
description: The name, aliases and switch of every command and subcommand, and words per language.
icon: square-terminal
---

| Key | Ships as |
|---|---|
| `jobs` | `/jobs`, `/job`, `/j` |
| `contracts` | `/contracts`, `/contract` |
| `uxmjobs` | `/uxmjobs`, `/jobsadmin` |

Every subcommand has a block of its own under `subcommands`, with a `name` and optional `aliases`. A word under a word is
keyed with a hyphen and written with a space: `boost-start { name = "boost start" }`, `zone-requirements-clear { name =
"zone requirements clear" }`. The operator's points subcommand is keyed `admin-points` and named `points`.

| Key | What it does |
|---|---|
| `name` | The label typed |
| `aliases` | Other labels |
| `enabled` | `false` and the command is never registered |
| `localized-aliases` | Words for the readers of one language |
| `subcommands` | One block per subcommand; `enabled = false` takes that word out |

A change needs a restart. A renamed subcommand keeps its node. `/contracts` is its own command so a server that wants
only jobs switches it off in one line.

## Words per language

A word in `localized-aliases` on a command works for anyone who types it, but only a player who reads that language
finds it in their command list. A word under a subcommand is offered to, and works for, the readers of that language
and the console only.

Related: [Commands](../commands/overview.md)
