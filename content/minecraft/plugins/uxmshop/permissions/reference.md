---
title: Reference
order: 601
description: Every permission node of uxmShop with its default and what it allows.
icon: list-checks
---

A player node is held by everybody, so its command stays visible and a player without it is refused with
`command.no-permission`. An operator node hides its command from anybody who lacks it.

## Players

| Node | Default | Allows |
|---|---|---|
| `uxmshop.use` | true | `/shop`, and `/shop open` with the section's node |
| `uxmshop.value` | true | `/value` |
| `uxmshop.sell.hand` | true | `/sell hand` and `/sell` |
| `uxmshop.sell.all` | true | `/sell all` |
| `uxmshop.sell.window` | true | `/sell window` |
| `uxmshop.myshop` | true | `/myshop` and its branches |
| `uxmshop.shops` | true | Opens a player shop somebody else runs; the owner reaches their own through `uxmshop.myshop`. It is also the prefix of the shop limit |

`/shop lang`, `lang set`, `lang reset`, `/myshop close` and `/myshop staff` need no node.

## Nodes you complete

| Node | Default | Gives |
|---|---|---|
| `uxmshop.section.<section>` | op (`section.*`) | The usual node to write in a section's `permission` |
| `uxmshop.rank.<rank>` | op (`rank.*`) | A rank written in `shop.ranks`, unless it names its own `permission` |
| `uxmshop.bypass.blacklist.<material>` | op (`bypass.blacklist`) | Trades one blacklisted material |
| `uxmshop.shops.<n>` | undeclared | Owns `n` player shops; `uxmshop.shops.*` is no limit; without one, `player-shops.limit` |

A section or line may also name `permission`, `buy-permission` and `sell-permission` nodes in its file; the hub shows a
section whose own `permission` is empty or held.

The hub, `/shop open <section>` and a shortcut line ask the same thing: the section's own `permission`, or nothing
when it names none.

## Staff

| Node | Default | Allows |
|---|---|---|
| `uxmshop.bypass.tax` | op | Sells with no tax |
| `uxmshop.editor` | op | `/shopedit` |
| `uxmshop.admin.shop` | op | `/uxmshop adminshop` |
| `uxmshop.admin.modifier` | op | `/uxmshop modifier` |
| `uxmshop.admin.iteminfo` | op | `/uxmshop iteminfo` |
| `uxmshop.admin.logs` | op | `/uxmshop logs` |
| `uxmshop.admin.reset` | op | `resetprice`, `resetstock`, `resetquota` |
| `uxmshop.admin.prune` | op | `prune` |
| `uxmshop.admin.doctor` | op | `doctor` |
| `uxmshop.admin.version` | op | `version` and the bare command |
| `uxmshop.admin.reload` | op | `reload` |
| `uxmshop.admin.language` | op | `/shop lang set-player`, `lang reset-player` |
| `uxmshop.admin.*` | op | Every `uxmshop.admin` node above |

Related: [Commands](../commands/overview.md)
