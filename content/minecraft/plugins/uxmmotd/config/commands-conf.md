---
title: commands.conf
order: 602
description: The name, aliases and switch of the command and each word under it.
icon: square-terminal
---

```hocon
commands {
  uxmmotd {
    name    = "uxmmotd"
    aliases = ["motd"]
    enabled = true
    subcommands {
      reload             { name = "reload" }
      version            { name = "version" }
      maintenance        { name = "maintenance" }
      maintenance-on     { name = "maintenance on" }
      maintenance-off    { name = "maintenance off" }
      maintenance-status { name = "maintenance status" }
      doctor             { name = "doctor" }
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
| `subcommands` | One block per word; `enabled = false` takes that word out |

A word under a word is written with a hyphen on the left and a space on the right: `maintenance-on` is the key under
which `maintenance on` is named. A change needs a restart. A renamed word keeps its node.

Related: [Commands](../commands/overview.md)
