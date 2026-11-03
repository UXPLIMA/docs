---
title: Watches
order: 404
description: Waiting for an item and being told when it is listed.
icon: eye
---

A watch is a player waiting for an item. In the [watch window](../menus/watch.md), the spyglass watches the item in your
hand:

| Click | Watches |
|---|---|
| Left | Any item of that type |
| Right | The same type with the same enchantments |
| Shift left | The same item in every detail |

A player watches one thing once: two clicks on the same item are one watch. When the item is listed, the watchers are
told, oldest first, up to `house.work.watches-told` (20) per listing. A click on a watch in the window stops it.

`house.max-watches-per-player` (50) limits watches per player. Node `uxmauction.watch`.

Related: [Watch window](../menus/watch.md)
