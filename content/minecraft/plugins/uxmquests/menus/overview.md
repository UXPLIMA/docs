---
title: Overview
order: 501
description: Every window, its file, its size and what opens it.
icon: layout-grid
---

| Window | File | Rows | Opened by |
|---|---|---|---|
| [Quests](quests.md) | `quests.conf` | 3 | `/quests` |
| [Board](quests.md#board) | `board.conf` | 4 | A group on the quests window |
| [Passes](passes.md) | `passes.conf` | 3 | The star on the quests window, while a pass runs |
| [Pass](passes.md#pass) | `pass.conf` | 6 | A pass on the passes window |

Every window is a file; move a slot, change a material, take a button out, and the plugin draws what you wrote. Delete
`quests.conf` and `/quests` writes the boards in chat instead. A Bedrock player gets each window as a form.

The quests window draws a star at slot 22 (`quests:passes`) while a pass runs; it opens the passes window, whose feather
at 22 (`quests:back`) comes back. `/quests pass` answers the same question in chat.

Related: [Menu files](../config/menu-files.md)
