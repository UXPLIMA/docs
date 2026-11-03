---
title: Admin log
order: 511
description: The audit log in a window, with one click to give an item back.
icon: scroll
---

`/uxmauction log [player]` as a player, node `uxmauction.admin.log`. Six rows; `menus/admin/log.conf`.

| Slot | Holds | Click |
|---|---|---|
| 0 to 44 | One line of the log per square, newest first | Gives the item of that line back, once |
| 46 | A writable book | What the log is about: everyone, or one player |

The click checks no node of its own: anyone who may open the log with `uxmauction.admin.log` may restore from it, even
without `uxmauction.admin.restore`.

Related: [Operators](../features/operators.md#the-log)
