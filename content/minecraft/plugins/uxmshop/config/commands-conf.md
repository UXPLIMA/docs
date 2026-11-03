---
title: commands.conf
order: 604
description: Renaming, aliasing and switching off every command and branch, and words per language.
icon: terminal
---

| Key | Name | Aliases |
|---|---|---|
| `shop` | `shop` | `shops` |
| `sell` | `sell` | |
| `value` | `value` | `worth` |
| `myshop` | `myshop` | `ms` |
| `uxmshop` | `uxmshop` | `shopadmin` |
| `shopedit` | `shopedit` | |

| Key | Meaning |
|---|---|
| `name` | The label a player types, without the slash |
| `aliases` | Other labels for the same command |
| `enabled` | false and the command is never registered |
| `subcommands` | Each branch by its key, with its own `name`, `aliases` and `enabled` |
| `localized-aliases` | Words per language tag |

## Words per language

```hocon
shop {
  name = "shop"
  localized-aliases { tr = ["market"] }
}
```

The tag is a language (`tr`) or a language and a country (`pt-BR`). The key on the left never changes. A word on a
command works for anyone who types it; only a player reading that language finds it among their commands. A word under
`subcommands` is shown to, and works for, readers of that language and the console. `/shopedit` also needs
`editor.enabled`. A change needs a restart.

Related: [Commands](../commands/overview.md)
