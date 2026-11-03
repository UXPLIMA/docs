---
title: Reference
order: 601
description: Every permission node of uxmMinions with its default and what it allows.
icon: list-checks
---

## Players

`/minions` needs no node.

## Nodes the files name

None of these is declared, so by default only operators hold them. Give them to the players who should have them.

| Node | Named in | Allows |
|---|---|---|
| `uxmminions.destination.sell` | `destinations.sell.permission` | Choosing the sell destination |
| `uxmminions.store.buy` | `menus/store.conf`, `menus/my-minions.conf` | Opening the store and its button |
| `uxmminions.skin.ember`, `.tide`, `.grove`, `.dusk` | `skins.conf` | Wearing that suit |
| `uxmminions.admin` | `menus/panel.conf` | Seeing the remove button on a panel |

A `permission` requirement in a type file, a `view` gate and a destination node are yours to name.

## Declared

| Node | Default | Allows |
|---|---|---|
| `uxmminions.limits.bypass` | op | Placing past every limit |
| `uxmminions.admin.*` | op | Everything an operator does |
| `uxmminions.admin.version` | op | `/uxmminions`, `version` |
| `uxmminions.admin.doctor` | op | `doctor` |
| `uxmminions.admin.reload` | op | `reload` |
| `uxmminions.admin.language` | op | `/minions lang set-player`, `lang reset-player` |
| `uxmminions.admin.editor` | op | `editor` and its windows |
| `uxmminions.admin.give` | op | `give` |
| `uxmminions.admin.remove` | op | `remove` |
| `uxmminions.admin.find` | op | `find` |
| `uxmminions.admin.open` | op | `open` |
| `uxmminions.admin.minions` | op | `minions` and the admin window |
| `uxmminions.admin.slots` | op | `slots` |
| `uxmminions.admin.ledger` | op | `ledger` |
| `uxmminions.admin.restore` | op | `restore` |
| `uxmminions.admin.migrate` | op | `migrate` |

`uxmminions.admin.*` declares no children, so a permission plugin that does not read wildcards grants nothing by it.

Related: [Commands](../commands/overview.md)
