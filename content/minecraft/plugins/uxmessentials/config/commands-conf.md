---
title: Renaming Commands
order: 1170
description: Renaming, re-aliasing and disabling any command in commands.conf.
icon: file-cog
---

`plugins/uxmEssentials/commands/commands.conf` renames, re-aliases or disables any command. It is one
global file, keyed by each command's stable id.

## How to Edit

1. Open `plugins/uxmEssentials/commands/commands.conf`.
2. Edit the `name`, `aliases`, or `enabled` value for the command you want to change.
3. Save.
4. Reload (see the tip at the bottom).

This file is HOCON: `#` comments, quoted strings, `[ … ]` lists, `{ … }` blocks. Keep braces and quotes
balanced.

## How the file is built

`commands.conf` is generated from the live command list the **first time** the plugin
runs, and then left untouched; your edits are safe across upgrades. Each command is one
block keyed by its **stable id**:

```hocon
commands {
  tpaccept {
    enabled = true
    name = "tpaccept"
    aliases = ["tpyes"]
  }
  tpdeny {
    enabled = true
    name = "tpdeny"
    aliases = ["tpno"]
  }
}
```

| Key | What it does |
|---|---|
| `enabled` | `false` drops the command entirely; it registers nothing. |
| `name` | The primary literal players type. Edit it to rename the command. |
| `aliases` | Extra literals that map to the same command. |

## Renaming a command

Change `name` to the word you want players to type. The id (the block key) stays the
same:

```hocon
commands {
  tpaccept {
    enabled = true
    name = "accept"           # players now type /accept
    aliases = ["tpyes", "yes"]
  }
}
```

<Callout type="warning" title="Permission nodes never change on rename">

Nodes are keyed to the command id, not to the name players type. Renaming `tpaccept` to `accept` leaves
`uxmessentials.tpa.use` alone, so your permission setup keeps working. Do not rename the block key
(`tpaccept`); edit only `name` and `aliases` inside it.

</Callout>

## Adding or replacing aliases

`aliases` is a plain list. Add entries to give players more ways to type the command, or
clear it to remove the alternates:

```hocon
commands {
  balance {
    enabled = true
    name = "balance"
    aliases = ["bal", "money", "cash"]   # added "cash"
  }
}
```

```hocon
commands {
  balance {
    enabled = true
    name = "balance"
    aliases = []                          # no alternates at all
  }
}
```

Avoid giving two commands the same name or alias: the first one registered wins and the
other silently loses that literal.

## Disabling a command

Set `enabled = false` to remove a command completely. It registers nothing: no literal,
no aliases, no tab-completion:

```hocon
commands {
  suicide {
    enabled = false           # /suicide is gone from this server
    name = "suicide"
    aliases = []
  }
}
```

Disabling a single command is different from disabling its whole module: the rest of the
module keeps working. To turn off an entire feature, use the module's `enabled` key
instead, see [Per-Module Config](per-module.md).

**Reload after editing.** Apply your changes with `/uxmess reload` (or restart). The command surface is
re-published on reload, so renames, new aliases and disabled commands take effect
immediately.

Related: [Commands Overview](../modules/index.md), [Per-Module Config](per-module.md), [Permission Reference](../permissions/reference.md)
