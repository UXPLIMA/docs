---
title: commands.conf
order: 507
description: The name, aliases, Turkish words and switch of every command and subcommand.
icon: square-terminal
---

```hocon
commands {
  login {
    name    = "login"
    aliases = ["l", "log"]
    enabled = true
    localized-aliases { tr = ["giris"] }
  }
  2fa {
    name    = "2fa"
    localized-aliases { tr = ["dogrulama"] }
    subcommands {
      setup { name = "setup", localized-aliases { tr = ["kur"] } }
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

Every command and its shipped words are in [Commands](../commands/overview.md). A change needs a restart.

## Words per language

A word in `localized-aliases` on a command works for anyone who types it, but only a player who reads that language
finds it in their command list. A word under a subcommand is offered to, and works for, the readers of that language and
the console only.

Related: [Commands](../commands/overview.md)
