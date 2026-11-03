---
title: commands.conf
order: 502
description: The name, aliases and switch of every command and subcommand.
icon: square-terminal
---

```hocon
commands {
  glow {
    name    = "glow"
    aliases = ["glowcolor", "glowcolour"]
    enabled = true
    subcommands {
      set { name = "set" }
    }
  }
}
```

| Key | What it does |
|---|---|
| the block name | `glow` or `uxmglow`. It never changes |
| `name` | The label a player types |
| `aliases` | Other labels |
| `enabled` | `false` and the command leaves the tree |
| `localized-aliases` | Words for the readers of one language |
| `subcommands` | One block per subcommand |

| Command | Ships as | Keys |
|---|---|---|
| `glow` | `/glow`, aliases `/glowcolor`, `/glowcolour` | `set`, `cycle`, `off`, `colours`, `lang`, `lang-set`, `lang-reset`, `lang-set-player`, `lang-reset-player` |
| `uxmglow` | `/uxmglow`, aliases `/uxmg`, `/glowadmin` | `reload`, `version`, `doctor` |

A change needs a restart. A renamed subcommand keeps its node.

## Words per language

A word in `localized-aliases` on a command works for anyone who types it, but only a player who reads that language
finds it in their command list. A word under a subcommand is offered to, and works for, the readers of that language
and the console only.

Related: [Commands](../commands/overview.md)
