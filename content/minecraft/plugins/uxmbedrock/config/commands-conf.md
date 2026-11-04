---
title: commands.conf
order: 607
description: The name, aliases and switch of every command and subcommand, and words per language.
icon: square-terminal
---

```hocon
commands {
  bedrock {
    name    = "bedrock"
    aliases = ["be"]
    enabled = true
    localized-aliases { tr = ["bedrockayar"] }
    subcommands {
      menus { name = "menus", localized-aliases { tr = ["menüler"] } }
    }
  }
}
```

| Key | What it does |
|---|---|
| the block name | The command's key. It never changes |
| `name` | The label a player types |
| `aliases` | Other labels |
| `enabled` | `false` and the command is never registered |
| `localized-aliases` | Words for the readers of one language |
| `subcommands` | One block per subcommand; `enabled = false` takes that word out and leaves the rest |

A word under a word is written with a hyphen on the left and a space on the right: `pack-build` is the key under
which `pack build` is named. A change needs a restart. A renamed subcommand keeps its node.

| Key | Ships as | Subcommand keys |
|---|---|---|
| `bedrock` | `/bedrock`, `/be`; Turkish `/bedrockayar` | `menus`, `feature`, `animated`, `lang`, `lang-set`, `lang-reset` |
| `menus` | `/menus`; Turkish `/menüler` | |
| `uxmbedrock` | `/uxmbedrock`, `/bedrockadmin` | `reload`, `version`, `doctor`, `status`, `pack-build`, `pack-report`, `pack-sync`, `items-unknown`, `skulls`, `menus`, `preview`, `gui-measure`, `gui-list`, `lang-set-player`, `lang-reset-player` |

The Turkish words shipped under `/bedrock` are `menüler`, `özellik`, `hareketli`, `dil`, `dil seç` and `dil sıfırla`.

## Words per language

A word in `localized-aliases` on a command works for anyone who types it, but only a player who reads that language
finds it in their command list. A word under a subcommand is offered to, and works for, the readers of that language
and the console only.

Related: [Commands](../commands/overview.md)
