---
title: Player commands
order: 302
description: Every /minions subcommand, with its arguments and what it does.
icon: user
---

No node is needed for anything on this page except the two that set another player's language.

| Command | What it does |
|---|---|
| `/minions`, `/minions list` | Opens [your minions](../menus/my-minions.md); one line per minion if the window file is gone |
| `/minions report` | What your minions have paid you, per type, out of the ledger |
| `/minions trust <id> <player>` | Lets an online player use one of your minions |
| `/minions untrust <id> <player>` | Takes them off its trust list |
| `/minions name <id> <name>` | Names one of your minions; `-` takes the name off |
| `/minions consume <eaten> <into>` | Feeds one minion to another of the same type and level, when `upgrades.consume-minion` is on |
| `/minions lang` | The language you read this plugin in |
| `/minions lang set <language>` | Read it in one language |
| `/minions lang reset` | Follow the client again |
| `/minions lang set-player <player> <language>` | `uxmminions.admin.language`: another player's language |
| `/minions lang reset-player <player>` | `uxmminions.admin.language`: clear another player's choice |

## trust and name

A name is typed, so these are commands and not buttons: a chest window has nowhere to type. The
[trusted window](../menus/trusted.md) lists who you trusted and takes them off. A name shows on the panel title, on the
hologram's first line and in `%uxmminions_minion_<id>_name%`; one that is too long is refused with the most it may be.

## consume

Both minions must be yours and of the same type and level. The eaten one is gone and the other rises one level, in one
transaction. The [upgrade window](../menus/upgrade.md) has a button that eats your least worked candidate instead. It
ships off and the setting is read on each call.

## report

One line per type, up to `ledger.report-lines` (10), from what the ledger recorded.

Related: [Admin commands](admin.md), [Placing and using](../features/placing-and-using.md)
