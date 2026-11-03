---
title: Locking
order: 404
description: Items that stay where they were put and are kept on death.
icon: lock
---

```hocon
restrictions {
  locked = true
}
```

A locked item stays where it was put: it cannot be moved, dragged, swapped, dropped or placed, and it is taken out of the
death drops instead of falling on the floor. A respawn trigger then hands it back.

An unlocked item behaves like any other item: a player may move it, drop it, or lose it on death. It still keeps its
mark, so `/uxmjoinitems clear` finds it.

Related: [Item files](../config/item-files.md)
