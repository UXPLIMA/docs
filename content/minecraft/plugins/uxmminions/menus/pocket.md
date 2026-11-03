---
title: Pocket
order: 404
description: The minions a player picked up and has not placed again.
icon: archive
---

`menus/pocket.conf`: five rows, redrawn every 20 ticks.

| Slot | Item | Click |
|---|---|---|
| 10 to 16, 19 to 25 | Each stored minion, with its level | Hands it back at the level it went away with |
| 22 | Barrier, "nothing here", under the list | |
| 36 | Arrow, back | Your minions |

A pickup into a full inventory used to leave a minion no window listed; this is that list. List source
`minions:stored`; action `minions:take-out`.

Related: [Placing and using](../features/placing-and-using.md#pickup-and-the-pocket)
