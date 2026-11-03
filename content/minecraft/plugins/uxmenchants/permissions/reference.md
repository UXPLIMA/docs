---
title: Reference
order: 601
description: Every permission node of uxmEnchants with its default and what it allows.
icon: list-checks
---

## Players

| Node | Default | Allows |
|---|---|---|
| `uxmenchants.use` | true | `/enchants`, every window, `info`, `list`, `limit`, `descriptions`, `lang`, `lang set`, `lang reset` |

A player whose rank had `uxmenchants.use` taken away still sees `/enchants` and is told they may not use it.

## Enchantments

| Node | Default | Allows |
|---|---|---|
| `uxmenchants.enchantment.blunted` | true | Use blunted |
| `uxmenchants.enchantment.curse-of-brittleness` | true | Use curse of brittleness |
| `uxmenchants.enchantment.cushioned` | true | Use cushioned |
| `uxmenchants.enchantment.ember` | true | Use ember |
| `uxmenchants.enchantment.prospector` | true | Use prospector |
| `uxmenchants.enchantment.scholar` | true | Use scholar |
| `uxmenchants.enchantment.sharpened` | true | Use sharpened |
| `uxmenchants.enchantment.smelter` | true | Use smelter |
| `uxmenchants.enchantment.steadfast` | true | Use steadfast |
| `uxmenchants.enchantment.thunderstruck` | true | Use thunderstruck |
| `uxmenchants.enchantment.tree-feller` | true | Use tree feller |
| `uxmenchants.enchantment.vein-miner` | true | Use vein miner |

An enchantment's node is `uxmenchants.enchantment.<id>`, or the `permission` its file writes; `""` opens it to everyone.
A new enchantment's node is not declared, so by default only operators hold it until you give it. Without it a player
cannot be given that enchantment at the table or the anvil, and an item that carries it does not fire for them and
draws it struck through.

## Table and limit nodes

| Node | Default | Allows |
|---|---|---|
| `<enchantment node>.table` | Not declared: operators only | Being offered that enchantment at the enchanting table, together with its node |
| `uxmenchants.limit.<number>` | Not declared | One item may carry that many of ours, up to `limit.maximum-from-node` (32) |

## Operators

| Node | Default | Allows |
|---|---|---|
| `uxmenchants.admin.version` | op | `/uxmenchants`, `/uxmenchants version` |
| `uxmenchants.admin.doctor` | op | `/uxmenchants doctor` |
| `uxmenchants.admin.reload` | op | `/uxmenchants reload` |
| `uxmenchants.admin.language` | op | `/enchants lang set-player`, `lang reset-player` |
| `uxmenchants.admin.give` | op | `give`, `giverandom` |
| `uxmenchants.admin.apply` | op | `apply` on the held item |
| `uxmenchants.admin.remove` | op | `remove` from the held item |
| `uxmenchants.admin.strip` | op | `strip <world>` |
| `uxmenchants.admin.export` | op | `export` of the held item |
| `uxmenchants.admin.import` | op | `import <lore\|registry>` |
| `uxmenchants.admin.editor` | op | `/uxmenchants editor` and its windows |

There is no parent node for the operator branch; give each node, or `uxmenchants.admin.*` through a permission plugin
that reads wildcards.

The descriptions the plugin declares for `admin.strip` ("off a held item"), `admin.export` ("to a file") and
`admin.import` ("content") are older than the commands; the table above says what each one does.

Related: [Commands](../commands/overview.md)
