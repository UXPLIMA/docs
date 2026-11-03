---
title: Admin commands
order: 203
description: Cancel, return, purge, log, restore, stats, lookup, lock, reload, version, doctor and another player's language.
icon: wrench
---

| Command | Node | What it does |
|---|---|---|
| `/uxmauction cancel <id>` | `uxmauction.admin.cancel` | Takes anyone's listing or order down. The house looks for a listing first, then an order. An auction with bids gives the money back before the item |
| `/uxmauction return <player>` | `uxmauction.admin.return` | Empties one player out of the house: what comes back goes into their mailbox |
| `/uxmauction purge <days>` | `uxmauction.admin.purge` | Deletes what closed more than that many days ago. Nothing on the shelf, held, or waiting for a player is reached |
| `/uxmauction log [player]` | `uxmauction.admin.log` | The newest `admin.log-lines` (10) lines of the audit log; a player reads it in the [log window](../menus/admin-log.md), where a click gives an item back |
| `/uxmauction restore <transaction>` | `uxmauction.admin.restore` | Gives the item of one logged line back, once |
| `/uxmauction lookup <listing>` | `uxmauction.admin.lookup` | Everything about one listing, whoever owns it |
| `/uxmauction stats` | `uxmauction.admin.stats` | The size of the house |
| `/uxmauction lock [on\|off]` | `uxmauction.admin.lock` | Closes or opens the house on every server; without a word, says which |
| `/uxmauction reload` | `uxmauction.admin.reload` | Reads the files again and swaps them in whole |
| `/uxmauction version` | `uxmauction.admin.version` | The version this server runs |
| `/uxmauction doctor` | `uxmauction.admin.doctor` | The `storage`, `windows` and `placeholders` checks |
| `/auction lang set-player <player> <language>` | `uxmauction.admin.language` | Sets the language another player reads |
| `/auction lang reset-player <player>` | `uxmauction.admin.language` | Clears it |

Every move an operator makes on somebody else's row is written to the log and, with the webhook on, posted.

Related: [Operators](../features/operators.md)
