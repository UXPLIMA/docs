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
| [Passes](passes.md) | `passes.conf` | 3 | A button with `open:passes` |
| [Pass](passes.md#pass) | `pass.conf` | 6 | A pass on the passes window |

Every window is a file; move a slot, change a material, take a button out, and the plugin draws what you wrote. Delete
`quests.conf` and `/quests` writes the boards in chat instead. A Bedrock player gets each window as a form.

<Callout type="warning" title="The passes window has no door">

No shipped window or command opens `passes.conf`; `/quests pass` answers in chat. Add a button to `quests.conf`:

```hocon
passes {
  slot = 22
  material = ENCHANTED_BOOK
  name = "Passes"
  click { left = ["open:passes"] }
}
```

and move the paging bar out of slot 22's row, or put the button in a free slot.

</Callout>

Related: [Menu files](../config/menu-files.md)
