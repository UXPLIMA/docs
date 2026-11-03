---
title: Storage
order: 506
description: A live view over a minion's stock, with take, toggle, mode and sort.
icon: package-open
---

`menus/storage.conf`: six rows, redrawn every 20 ticks.

| Slot | Item | Click |
|---|---|---|
| 0 to 44 | Each material held: amount and how many collected | Left: the current mode. Shift left: take the stack. Right: stop or start collecting it |
| 45 | Arrow, back | The panel |
| 47 | Compass: the order | Next order: most, least, material |
| 49 | Hopper | Takes everything out |
| 53 | Comparator: the mode | Next mode: take one, take all, drop all |

Every take is its own transaction and nothing is written back when the window closes, so nothing duplicates while the
minion works. A sort moves nothing: the stock is a merged table, so an order is a view.

| Action | Does |
|---|---|
| `minions:take-one`, `minions:take-all`, `minions:drop-all` | Take one, the stack, or everything |
| `minions:toggle-item` | Stops the minion collecting that material, or starts it again |
| `minions:storage-row`, `minions:storage-mode` | Runs the mode the button shows; walks to the next mode |
| `minions:sort`, `minions:sort most\|least\|material` | Walks every order, or sets one |
| `minions:panel` | Back to the panel |

Related: [Output](../features/output.md)
