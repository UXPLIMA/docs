---
title: commands.conf
order: 606
description: Names, aliases, switches, the skill shortcuts and words per language.
icon: terminal
---

| Key | Name | Aliases |
|---|---|---|
| `skills` | `skills` | `skill`, `sk` |
| `uxmskills` | `uxmskills` | `skillsadmin` |
| `skillsedit` | `skillsedit` | `seedit` |
| `shortcuts` | one per skill | |

Each command has `name`, `aliases`, `enabled` and `subcommands`. Two ways to switch a branch off:

```hocon
subcommands { doctor { enabled = false } }   # the word leaves the command tree
subcommands { doctor = false }               # the word stays and the command refuses it
```

## Words per language

`localized-aliases { tr = ["beceri"] }` adds words for readers of one language. A word on a command works for anyone
who types it; only a player reading that language finds it among their commands. A change needs a restart; the
shortcut family is built again from the skill files every time.

Related: [Commands](../commands/overview.md#skill-shortcuts)
