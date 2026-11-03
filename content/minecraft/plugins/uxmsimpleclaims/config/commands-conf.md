---
title: commands.conf
order: 503
description: Command and branch words, aliases and the Turkish words.
icon: terminal
---

```hocon
commands {
  claim {
    name    = "claim"
    aliases = ["claims"]
    localized-aliases { tr = ["arazi"] }
    enabled = true
    subcommands {
      here { name = "here", localized-aliases { tr = ["burasi"] } }
    }
  }
  claimadmin {
    name    = "claimadmin"
    aliases = ["uxmsimpleclaims"]
    localized-aliases { tr = ["araziyonetim"] }
  }
}
```

## Words per language

Every branch has a Turkish word; they are listed on the [command pages](../commands/overview.md). A word on a command
works for anyone who types it; only a player reading that language finds it among their commands. A word under
`subcommands` is shown to, and works for, readers of that language and the console. The key on the left never changes.
A change needs a restart.

Related: [Commands](../commands/overview.md)
