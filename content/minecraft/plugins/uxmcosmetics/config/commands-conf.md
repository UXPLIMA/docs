---
title: commands.conf
order: 605
description: The name, aliases and switch of every command and subcommand.
icon: square-terminal
---

Every command and subcommand can be renamed, given aliases or turned off here. A change needs a restart.

```hocon
commands {
  cosmetics {
    name    = "cosmetics"
    aliases = ["cosmetic", "wardrobe"]
    enabled = true
    localized-aliases { tr = ["kozmetik"] }
    subcommands {
      open { name = "open" }
    }
  }
}
```

| Key | What it does |
|---|---|
| the block name | The command's key, `cosmetics` or `uxmcosmetics`. It never changes |
| `name` | The label a player types |
| `aliases` | Other labels |
| `enabled` | `false` and the command is never registered |
| `localized-aliases` | Words for the readers of one language |
| `subcommands` | One block per subcommand, keyed as below |

| Command | Ships as | Keys |
|---|---|---|
| `cosmetics` | `/cosmetics`, aliases `/cosmetic`, `/wardrobe` | `open`, `boxes`, `equip`, `unequip`, `unequip-all`, `lang`, `lang-set`, `lang-reset`, `lang-set-player`, `lang-reset-player` |
| `uxmcosmetics` | `/uxmcosmetics`, aliases `/uxmcos`, `/cosmeticsadmin` | `grant`, `revoke`, `reload`, `cleanup`, `version`, `station-add`, `station-set`, `station-show`, `station-remove`, `station`, `station-list`, `doctor` |

A subcommand under a subcommand is keyed with a hyphen and named with a space: `unequip-all` is named `unequip all`.
A renamed subcommand keeps its node.

## Words per language

A word in `localized-aliases` on a command works for anyone who types it, but only a player who reads that language
finds it in their command list. A word under a subcommand is offered to, and works for, the readers of that language
and the console only. A word may use the letters of its own language.

Related: [Commands](../commands/overview.md)
