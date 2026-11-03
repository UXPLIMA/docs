---
title: Reference
order: 701
description: Every permission node of uxmCosmetics with its default and what it allows.
icon: list-checks
---

## Players

| Node | Default | Allows |
|---|---|---|
| `uxmcosmetics.use` | everyone | The wardrobe and `/cosmetics` subcommands |
| `uxmcosmetics.language` | everyone | `/cosmetics lang`, `lang set`, `lang reset` |

A player who lost one of these still sees the command and is told `command.no-permission`.

## Pieces and boxes

| Node | Default | Gives |
|---|---|---|
| `uxmcosmetics.<family>.<id>` | nobody | One piece, as if owned. A piece's `permission` replaces it |
| `uxmcosmetics.box.<id>` | nobody | Opening one box. A box's `permission` replaces it |
| `uxmcosmetics.*` | op | Every piece and every box, while `permissions.wildcard = true` |
| `uxmcosmetics.box.*` | op | Declared, but lists no children and is not read by the plugin: grant each box's node |

<Callout type="warning" title="Boxes are closed by default">

No player holds a box node by default, so no player can open a box until you grant one:
`lp group default permission set uxmcosmetics.box.common true`.

</Callout>

## Operators

| Node | Allows |
|---|---|
| `uxmcosmetics.admin.*` | Declared as the family, `op`. It lists no children: grant the nodes below, or use a permission plugin's wildcard |
| `uxmcosmetics.admin.grant` | `/uxmcosmetics grant` |
| `uxmcosmetics.admin.revoke` | `/uxmcosmetics revoke` |
| `uxmcosmetics.admin.station` | Every `station` subcommand |
| `uxmcosmetics.admin.cleanup` | `/uxmcosmetics cleanup` |
| `uxmcosmetics.admin.doctor` | `/uxmcosmetics doctor` |
| `uxmcosmetics.admin.version` | `/uxmcosmetics`, `/uxmcosmetics version` |
| `uxmcosmetics.admin.reload` | `/uxmcosmetics reload` |
| `uxmcosmetics.admin.language` | `/cosmetics lang set-player`, `lang reset-player` |

All are `op` by default.

## LuckPerms examples

```
lp group default permission set uxmcosmetics.box.common true
lp group vip permission set uxmcosmetics.box.rare true
lp group vip permission set uxmcosmetics.halo.angel_halo true
lp group mvp permission set uxmcosmetics.pet.* true
lp group helper permission set uxmcosmetics.admin.grant true
```

`uxmcosmetics.pet.*` works through LuckPerms' own wildcards, which LuckPerms resolves.

Related: [Getting cosmetics](../features/getting-cosmetics.md), [Mystery boxes](../features/mystery-boxes.md)
