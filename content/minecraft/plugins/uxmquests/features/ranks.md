---
title: Ranks
order: 410
description: Bigger boards and more rerolls by a number on a permission node.
icon: award
---

```hocon
ranks {
  board-size = "uxmquests.count."
  rerolls    = "uxmquests.rerolls."
}
```

| Node | Gives |
|---|---|
| `uxmquests.count.<n>` | A board of `n` quests in every group |
| `uxmquests.count.<group>.<n>` | A board of `n` in that group only |
| `uxmquests.rerolls.<n>` | `n` rerolls per board |

The highest number a player holds wins, and the number in the group file is the floor. The plugin reads a number off
the node and never learns your group names.

Related: [Permissions](../permissions/reference.md#numbers-on-a-node)
