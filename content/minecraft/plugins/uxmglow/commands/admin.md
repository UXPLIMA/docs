---
title: Operator commands
order: 203
description: Every /uxmglow subcommand and its permission.
icon: shield
---

`/uxmglow`, aliases `/uxmg` and `/glowadmin`. Every node is `op` by default.

| Command | What it does | Permission |
|---|---|---|
| `/uxmglow` | Says to add `help` | `uxmglow.admin.version` |
| `/uxmglow reload` | Re-reads `config.conf`, the picker and the language files, and says how long it took | `uxmglow.admin.reload` |
| `/uxmglow version` | Reports the version | `uxmglow.admin.version` |
| `/uxmglow doctor` | The storage, named by its backend, place, prefix and schema version, then the windows and the placeholders | `uxmglow.admin.doctor` |

A reload is swapped in whole, so it is never half applied; a file that does not parse answers `admin.reload-failed`
and the server keeps what worked. `storage` and `commands.conf` need a restart.

The language of another player is set under `/glow lang set-player`, on the [player root](player.md).

Related: [Overview](overview.md)
