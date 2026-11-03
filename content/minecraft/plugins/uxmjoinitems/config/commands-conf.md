---
title: commands.conf
order: 604
description: The name, aliases and switch of the command and each subcommand, and words per language.
icon: square-terminal
---

```hocon
commands {
  uxmjoinitems {
    name    = "uxmjoinitems"
    aliases = ["joinitems", "uxmji"]
    enabled = true
    subcommands {
      give              { name = "give" }
      giveall           { name = "giveall" }
      clear             { name = "clear" }
      reload            { name = "reload" }
      version           { name = "version" }
      doctor            { name = "doctor" }
      lang              { name = "lang" }
      lang-set          { name = "lang set" }
      lang-reset        { name = "lang reset" }
      lang-set-player   { name = "lang set-player" }
      lang-reset-player { name = "lang reset-player" }
    }
  }
}
```

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
