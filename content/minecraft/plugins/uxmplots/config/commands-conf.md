---
title: commands.conf
order: 614
description: Renaming, aliasing and switching off every command and branch, and words per language.
icon: terminal
---

Three commands, each renameable without touching the code:

| Key | Name | Aliases |
|---|---|---|
| `plot` | `plot` | `plots` |
| `contest` | `contest` | `contests` |
| `uxmplots` | `uxmplots` | `plotadmin` |

| Key | Meaning |
|---|---|
| `name` | The label a player types, without the slash |
| `aliases` | Other labels for the same command |
| `enabled` | false and the command is never registered |
| `subcommands` | Each branch by its key, with its own `name`, `aliases` and `enabled` |
| `localized-aliases` | Words per language tag: `localized-aliases { tr = ["arsa"] }` |

## Switching a branch off

Two ways:

```hocon
subcommands { doctor { enabled = false } }   # the word leaves the command tree
subcommands { doctor = false }               # the word stays and the command refuses it
```

## Words per language

```hocon
plot {
  name = "plot"
  localized-aliases { tr = ["arsa"] }
}
```

The tag is a language (`tr`) or a language and a country (`pt-BR`). The key on the left never changes. A word on a command works for anyone who types it; only a player reading that
language finds it among their commands. A word under `subcommands` is shown to, and works for, readers of that language
and the console. A branch not listed keeps its name from the code. A change needs a restart.

Related: [Commands](../commands/overview.md)
