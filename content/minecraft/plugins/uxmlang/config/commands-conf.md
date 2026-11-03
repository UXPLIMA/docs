---
title: commands.conf
order: 502
description: The name, aliases and switch of both commands and their subcommands, and words per language.
icon: square-terminal
---

```hocon
commands {
  language {
    name    = "lang"
    aliases = ["language", "dil"]
    enabled = true
    subcommands {
      list  { name = "list" }
      set   { name = "set" }
      reset { name = "reset" }
    }
  }
  uxmlang {
    name    = "uxmlang"
    aliases = ["langadmin"]
    enabled = true
    subcommands {
      reload  { name = "reload" }
      version { name = "version" }
      get     { name = "get" }
      set     { name = "set" }
      reset   { name = "reset" }
      doctor  { name = "doctor" }
    }
  }
}
```

| Key | What it does |
|---|---|
| the block name | `language` or `uxmlang`. It never changes |
| `name` | The label a player types |
| `aliases` | Other labels |
| `enabled` | `false` and the command is never registered |
| `localized-aliases` | Words for the readers of one language: `localized-aliases { tr = ["dil"] }` |
| `subcommands` | One block per subcommand; `enabled = false` takes that word out |

A change needs a restart. A renamed subcommand keeps its node.

## Words per language

A word in `localized-aliases` on a command works for anyone who types it, but only a player who reads that language
finds it in their command list. A word under a subcommand is offered to, and works for, the readers of that language
and the console only.

Related: [Commands](../commands/overview.md)
