---
title: commands.conf
order: 504
description: Renaming, aliasing and switching off every command and branch, and words per language.
icon: terminal
---

| Key | Name | Aliases |
|---|---|---|
| `quests` | `quests` | `quest`, `q` |
| `uxmquests` | `uxmquests` | `uxmq`, `questsadmin` |

Branches of `quests`: `start`, `claim`, `reroll` (alias `swap`), `lang` (alias `language`), `lang-set`, `lang-reset`,
`pass`, `pass-claim`. Branches of `uxmquests`: `lang-set-player`, `lang-reset-player`, `reload`, `version`, `types`,
`problems`, `trigger`, `totals`, `finish`, `reset-board`, `reset-totals`, `pass-xp`, `pass-booster`, `pass-top`,
`doctor`.

| Key | Meaning |
|---|---|
| `name` | The label a player types, without the slash |
| `aliases` | Other labels for the same command |
| `enabled` | false and the command is never registered |
| `subcommands` | Each branch by its key, with its own `name` and `aliases` |
| `localized-aliases` | Words per language tag |

## Words per language

```hocon
quests {
  name = "quests"
  localized-aliases { tr = ["gorevler"] }
}
```

The tag is a language (`tr`) or a language and a country (`pt-BR`). The key on the left never changes. A word on a
command works for anyone who types it; only a player reading that language finds it among their commands. A word under
`subcommands` is shown to, and works for, readers of that language and the console. A change needs a restart: the
server builds its command tree once.

Related: [Commands](../commands/overview.md)
