---
title: Admin commands
order: 302
description: Reload, version, maintenance and doctor, with their nodes and replies.
icon: wrench
---

| Command | Node | What it does |
|---|---|---|
| `/uxmmotd` | `uxmmotd.admin.version` | Paper: a line pointing at `help` |
| `/uxmmotd help [page]` | | Paper: the subcommands the sender may use |
| `/uxmmotd version` | `uxmmotd.admin.version` | The running version |
| `/uxmmotd reload` | `uxmmotd.admin.reload` | Reads `config.conf`, the theme, the messages and the icons again, and says how long it took |
| `/uxmmotd maintenance` | `uxmmotd.admin.maintenance` | Whether maintenance is on |
| `/uxmmotd maintenance on` | `uxmmotd.admin.maintenance` | Closes the server; says so when it was already on |
| `/uxmmotd maintenance off` | `uxmmotd.admin.maintenance` | Opens it again; says so when it was already off |
| `/uxmmotd maintenance status` | `uxmmotd.admin.maintenance` | Whether maintenance is on |
| `/uxmmotd doctor` | `uxmmotd.admin.doctor` | The `content` and `icons` checks, one line each |

A reload is applied as a whole or not at all: when a file is invalid the previous configuration stays in use and the
console holds the reason. `commands.conf` needs a restart.

Related: [Maintenance](../features/maintenance.md)
