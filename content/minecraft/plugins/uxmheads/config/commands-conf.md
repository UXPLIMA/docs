---
title: commands.conf
order: 602
description: The name, aliases and switch of every command and subcommand.
icon: square-terminal
---

```hocon
commands {
  uxmheads {
    name    = "uxmheads"
    aliases = ["heads", "uxmh"]
    enabled = true
    subcommands {
      preview { name = "preview" }
    }
  }
}
```

| Key | What it does |
|---|---|
| the block name | `uxmheads`. It never changes |
| `name`, `aliases` | The labels |
| `enabled` | `false` and the command is never registered |
| `localized-aliases` | Words for the readers of one language |
| `subcommands` | `preview`, `refresh`, `welcome`, `reload`, `version`, `doctor`, `lang`, `lang-set`, `lang-reset`, `lang-set-player`, `lang-reset-player` |

A change needs a restart. A renamed subcommand keeps its node.

## Words per language

A word in `localized-aliases` on a command works for anyone who types it, but only a player who reads that language
finds it in their command list. A word under a subcommand is offered to, and works for, the readers of that language
and the console only.

Related: [Commands](../commands/overview.md)
