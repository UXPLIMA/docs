---
title: commands.conf
order: 604
description: The name, aliases and switch of every command and subcommand.
icon: square-terminal
---

Every command and subcommand can be renamed, given aliases or turned off here, without touching the code. A change
needs a restart: the server builds its command tree once, at start.

```hocon
commands {
  crates {
    name    = "crates"
    aliases = ["crate", "cr"]
    enabled = true
    localized-aliases { tr = ["kasa"] }
    subcommands {
      open { name = "open", localized-aliases { tr = ["ac"] } }
    }
  }
}
```

## The keys

| Key | What it does |
|---|---|
| the block name | The command's key, `crates` or `uxmcrates`. It never changes |
| `name` | The label a player types, without the slash |
| `aliases` | Other labels that reach the same command |
| `enabled` | `false` and the command is never registered |
| `localized-aliases` | Words for the readers of one language, a list per language tag (`tr`, `pt-BR`) |
| `subcommands` | One block per subcommand: its fixed key on the left, `name`, `enabled` and `localized-aliases` inside |

A subcommand under a subcommand is keyed with a hyphen and named with a space: `key-give` is the key under which
`key give` is named.

## The subcommand keys

| Command | Ships as | Keys |
|---|---|---|
| `crates` | `/crates`, aliases `/crate`, `/cr` | `open`, `preview`, `keys`, `claim`, `send`, `massopen`, `cost`, `milestones` |
| `uxmcrates` | `/uxmcrates`, aliases `/cratesadmin`, `/uxmcr` | `key`, `key-give`, `key-giveitem`, `key-take`, `key-set`, `key-giveall`, `key-giveoffline`, `key-ledger`, `place`, `tool`, `envoy-start`, `envoy-stop`, `envoy-list`, `remove`, `forceopen`, `reset`, `reset-cooldown`, `reset-player-limit`, `reset-global-limit`, `reroll`, `reroll-give`, `log`, `editor`, `migrate`, `doctor`, `reload`, `version`, `lang`, `lang-set`, `lang-reset`, `lang-set-player`, `lang-reset-player` |

## Words per language

A word in `localized-aliases` on a command works for anyone who types it, but only a player who reads that language
finds it in their command list. A word under a subcommand is offered to, and works for, the readers of that language
and the console only. A word may use the letters of its own language: `kasa`, `ящик`.

## Notes

- **A permission belongs to the command, not its name.** A renamed subcommand keeps its node.
- **Turning off a subcommand** removes it from the tree and from tab completion; the rest of the command stays.
- **The bare roots** cannot be turned off on their own: `/crates` opens the crate list and `/uxmcrates` points to
  `help` while the root is enabled.

Related: [Commands](../commands/overview.md), [Languages](languages.md)
