---
title: Renaming Commands
order: 1170
description: plugins/uxmEssentials/commands/commands.conf lets you rename, re-alias,
  or disable any command without touching code or waiting for an update. It is one
  global file, keyed by each command's stable id.
---

## How to Edit

1. Open `plugins/uxmEssentials/commands/commands.conf`.
2. Edit the `name`, `aliases`, or `enabled` value for the command you want to change.
3. Save.
4. Reload (see the tip at the bottom).

<Callout type="warning" title="HOCON, not YAML">

This file is HOCON: `#` comments, quoted strings, `[ … ]` lists, `{ … }` blocks.
Keep braces and quotes balanced.

</Callout>

---

## How the file is built

`commands.conf` is generated from the live command list the **first time** the plugin
runs, and then left untouched — your edits are safe across upgrades. Each command is one
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
| `enabled` | `false` drops the command entirely — it registers nothing. |
| `name` | The primary literal players type. Edit it to rename the command. |
| `aliases` | Extra literals that map to the same command. |

---

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

<Callout type="info" title="Permission nodes never change on rename">

Permission nodes are keyed to the **command id**, not to the name players type.
Renaming `tpaccept` to `accept` does not change `uxmessentials.tpa.use` — your
permission setup keeps working. Renaming is purely cosmetic. Do **not** rename the
block key (`tpaccept`); only edit `name` and `aliases` inside it.

</Callout>

---

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

Avoid giving two commands the same name or alias — the first one registered wins and the
other silently loses that literal.

---

## Disabling a command

Set `enabled = false` to remove a command completely. It registers nothing — no literal,
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
instead — see [Per-Module Config](per-module.md).

---

<Callout type="tip" title="Reload after editing">

Apply your changes with `/uxmess reload` (or restart). The command surface is
re-published on reload, so renames, new aliases and disabled commands take effect
immediately.

</Callout>

---

## Next Steps

- [Commands Overview](../commands/overview.md) — every command and its default aliases.
- [Per-Module Config](per-module.md) — disable a whole feature instead of one command.
- [Permission Reference](../permissions/reference.md) — the id-keyed nodes that stay
  stable across renames.
