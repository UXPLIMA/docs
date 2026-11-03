---
title: commands.conf
order: 603
description: The name, aliases and switch of every command and subcommand, and words per language.
icon: square-terminal
---

```hocon
commands {
  lobby {
    name    = "lobby"
    aliases = ["lobbies", "hub"]
    enabled = true
    subcommands {
      lang              { name = "lang" }
      lang-set          { name = "lang set" }
      lang-reset        { name = "lang reset" }
      lang-set-player   { name = "lang set-player" }
      lang-reset-player { name = "lang reset-player" }
    }
  }
  quicklobby {
    name    = "quicklobby"
    aliases = ["ql"]
    enabled = true
  }
  uxmmultilobby {
    name    = "uxmmultilobby"
    aliases = ["uxmml", "lobbyadmin"]
    enabled = true
    subcommands {
      reload  { name = "reload" }
      version { name = "version" }
      doctor  { name = "doctor" }
    }
  }
}
```

| Key | What it does |
|---|---|
| the block name | `lobby`, `quicklobby` or `uxmmultilobby`. It never changes |
| `name` | The label a player types |
| `aliases` | Other labels |
| `enabled` | `false` and the command is never registered |
| `localized-aliases` | Words for the readers of one language: `localized-aliases { tr = ["lobiler"] }` |
| `subcommands` | One block per subcommand; `enabled = false` takes that word out |

A change needs a restart. A renamed subcommand keeps its node.

## Words per language

A word in `localized-aliases` on a command works for anyone who types it, but only a player who reads that language
finds it in their command list. A word under a subcommand is offered to, and works for, the readers of that language
and the console only.

Related: [Commands](../commands/overview.md)
