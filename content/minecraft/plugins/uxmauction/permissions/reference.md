---
title: Reference
order: 601
description: Every permission node of uxmAuction with its default and what it allows.
icon: list-checks
---

## Players

| Node | Default | Allows |
|---|---|---|
| `uxmauction.use` | everyone | Opening the house |
| `uxmauction.sell` | everyone | `sell`: putting an item up for sale |
| `uxmauction.auction` | everyone | `auction`: putting an item up for bids |
| `uxmauction.bid` | everyone | `bid` |
| `uxmauction.buy` | everyone | `buy` |
| `uxmauction.order` | everyone | `order`: the house buys for you and holds the money |
| `uxmauction.fill` | everyone | `fill`: selling into somebody's order |
| `uxmauction.cancel` | everyone | `cancel`: your own listing or order |
| `uxmauction.reprice` | everyone | `reprice` |
| `uxmauction.claim` | everyone | `claim` |
| `uxmauction.mailbox` | everyone | `mailbox` |
| `uxmauction.mine` | everyone | `mine`, `bids`, `orders` |
| `uxmauction.watch` | everyone | `watch` |
| `uxmauction.search` | everyone | `search` |
| `uxmauction.history` | everyone | `history` |
| `uxmauction.profile` | everyone | `profile` |
| `uxmauction.language` | everyone | `lang`, `lang set`, `lang reset` |

A player who lost one of these still sees the subcommand and is told `command.no-permission`.

## Fee groups

The nodes under `groups` in a fee, such as `uxmauction.fee.vip`, are yours to name. The plugin does not declare them.

## Operators

| Node | Default | Allows |
|---|---|---|
| `uxmauction.admin.*` | op | Declared as the family. It lists no children: grant the nodes below, or use a permission plugin's wildcard |
| `uxmauction.admin.cancel` | op | `cancel`: anyone's listing or order |
| `uxmauction.admin.return` | op | `return` |
| `uxmauction.admin.restore` | op | `restore`. A click in the log window needs only `uxmauction.admin.log` |
| `uxmauction.admin.lock` | op | `lock` |
| `uxmauction.admin.purge` | op | `purge` |
| `uxmauction.admin.lookup` | op | `lookup` |
| `uxmauction.admin.log` | op | `log` |
| `uxmauction.admin.stats` | op | `stats` |
| `uxmauction.admin.doctor` | op | `doctor` |
| `uxmauction.admin.version` | op | `version` |
| `uxmauction.admin.reload` | op | `reload` |
| `uxmauction.admin.language` | op | `lang set-player`, `lang reset-player` |

Related: [Commands](../commands/overview.md)
