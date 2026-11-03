---
title: Pickup
order: 511
description: The question asked before a minion is picked up.
icon: hand
---

`menus/pickup.conf`: three rows. Nothing is picked up until the player clicks yes.

| Slot | Item | Click |
|---|---|---|
| 11 | Lime wool, yes | Picks the minion up into the inventory, and closes |
| 15 | Red wool, no | Back to the panel |

Actions `minions:pickup-confirm` and `minions:panel`. Both are left clicks.

Related: [Placing and using](../features/placing-and-using.md#pickup-and-the-pocket)
