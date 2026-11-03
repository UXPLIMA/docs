---
title: commands.conf
order: 502
description: The name, aliases and switch of every command and subcommand, and words per language.
icon: square-terminal
---

Every command and subcommand can be renamed, given aliases or turned off here, without touching the code. A change
needs a restart: the server builds its command tree once, at start.

```hocon
commands {
  hide {
    name    = "hide"
    aliases = ["playerhider", "ph"]
    enabled = true
    localized-aliases { tr = ["gizle"] }
    subcommands {
      state  { name = "state", localized-aliases { tr = ["durum"] } }
      status { name = "status" }
    }
  }
}
```

## The keys

| Key | What it does |
|---|---|
| the block name | The command's key, `hide` or `uxmplayerhider`. It never changes |
| `name` | The label a player types, without the slash |
| `aliases` | Other labels that reach the same command |
| `enabled` | `false` and the command is never registered |
| `localized-aliases` | Words for the readers of one language, a list per language tag (`tr`, `pt-BR`) |
| `subcommands` | One block per subcommand: its fixed key on the left, `name`, `enabled` and `localized-aliases` inside |

A subcommand under a subcommand is keyed with a hyphen and named with a space: `lang-set` is the key under which
`lang set` is named.

## The subcommand keys

| Command | Ships as | Keys |
|---|---|---|
| `hide` | `/hide`, aliases `/playerhider`, `/ph` | `state`, `status`, `lang`, `lang-set`, `lang-reset`, `lang-set-player`, `lang-reset-player` |
| `uxmplayerhider` | `/uxmplayerhider`, aliases `/uxmph`, `/hideadmin` | `reload`, `version`, `doctor` |

## Words per language

A word in `localized-aliases` on a command works for anyone who types it, but only a player who reads that language
finds it in their command list. A word under a subcommand is offered to, and works for, the readers of that language
and the console only. The language a player reads is the one they chose, or their client's. A word may use the
letters of its own language: `скрыть`, `gizle`.

## Notes

- **A permission belongs to the command, not its name.** A renamed subcommand keeps its node.
- **Turning off a subcommand** removes it from the tree and from tab completion; the rest of the command stays.
- **The bare roots** cannot be turned off on their own: `/hide` moves to the next state and `/uxmplayerhider` points
  to `help` while the root is enabled.

Related: [Commands](../commands/overview.md), [Languages](languages.md)
