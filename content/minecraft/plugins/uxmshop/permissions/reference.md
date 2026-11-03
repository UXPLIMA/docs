---
title: Reference
order: 701
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
| `uxmshop.shops` | true | Declared as browsing player shops; nothing asks it. It is the prefix of the shop limit |

`/shop lang`, `lang set`, `lang reset`, `/myshop close` and `/myshop staff` need no node.

## Nodes you complete

| Node | Default | Gives |
|---|---|---|
| `uxmshop.section.<section>` | op (`section.*`) | Opens that section with `/shop open <section>` |
| `uxmshop.rank.<rank>` | op (`rank.*`) | A rank written in `shop.ranks`, unless it names its own `permission` |
| `uxmshop.bypass.blacklist.<material>` | op (`bypass.blacklist`) | Trades one blacklisted material |
| `uxmshop.shops.<n>` | undeclared | Owns `n` player shops; `uxmshop.shops.*` is no limit; without one, `player-shops.limit` |

A section or line may also name `permission`, `buy-permission` and `sell-permission` nodes in its file; the hub shows a
section whose own `permission` is empty or held.

<Callout type="warning" title="/shop open asks a node the hub does not">

The hub opens any section whose file names no `permission`, but `/shop open <section>` always asks
`uxmshop.section.<section>`, which only operators hold. Grant `uxmshop.section.<section>` (or the wildcard) to players
who should open sections by command, from a sign or from another plugin's button.

</Callout>

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
| `uxmshop.admin.*` | op | Declared with no children: grant the nodes above, or use your permission plugin's wildcard |

Related: [Commands](../commands/overview.md)
