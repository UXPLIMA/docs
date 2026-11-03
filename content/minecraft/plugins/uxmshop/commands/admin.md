---
title: Admin commands
order: 304
description: Every /uxmshop branch, its node and what it does.
icon: shield
---

| Command | Node | Does |
|---|---|---|
| `/uxmshop` | `uxmshop.admin.version` | Says what the command is for |
| `/uxmshop version` | `uxmshop.admin.version` | The running version |
| `/uxmshop reload` | `uxmshop.admin.reload` | Reads every file again |
| `/uxmshop doctor` | `uxmshop.admin.doctor` | Checks storage, windows and placeholders |
| `/uxmshop logs <player> <page>` | `uxmshop.admin.logs` | What one player traded, newest first |
| `/uxmshop logs direction <player> <direction> <page>` | `uxmshop.admin.logs` | The same, for one direction |
| `/uxmshop prune <days>` | `uxmshop.admin.prune` | Deletes log rows older than that |
| `/uxmshop adminshop` | `uxmshop.admin.shop` | Opens a [server shop](../features/player-shops.md#server-shops) on the container you look at; players only |
| `/uxmshop iteminfo` | `uxmshop.admin.iteminfo` | The item in your hand as a block to paste; players only |
| `/uxmshop modifier all <player> <factor>` | `uxmshop.admin.modifier` | A price multiplier for one player over the whole shop |
| `/uxmshop modifier section <player> <section> <factor>` | `uxmshop.admin.modifier` | The same for one section |
| `/uxmshop modifier line <player> <section> <line> <factor>` | `uxmshop.admin.modifier` | The same for one line |
| `/uxmshop modifier clear <player>` | `uxmshop.admin.modifier` | Removes every modifier of a player |
| `/uxmshop resetprice <section> <line>` | `uxmshop.admin.reset` | Puts a line back on its base price |
| `/uxmshop resetstock <section> <line> <direction>` | `uxmshop.admin.reset` | Resets the server stock of a line in one direction |
| `/uxmshop resetquota <player> <section> <line> <direction>` | `uxmshop.admin.reset` | Resets one player's allowance on a line |

Related: [Permissions](../permissions/reference.md)
