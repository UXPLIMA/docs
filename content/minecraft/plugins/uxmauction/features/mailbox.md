---
title: The mailbox
order: 305
description: What waits for a player, and taking it out.
icon: inbox
---

Everything the house owes a player waits in their mailbox until they take it:

| Row | Comes from |
|---|---|
| An item | A listing that expired or was cancelled, an auction won, an order filled, an operator's return |
| Money | A sale in a currency that cannot pay a player who is away, an outbid refund, an order cancelled |

`/auction mailbox` opens the [mailbox window](../menus/mailbox.md); a click takes one row, and `/auction claim <row>` does
the same by hand. An item currency and an experience currency live in the player, so a seller paid in them while away is
paid out of the mailbox when they return.

`%uxmauction_my_mailbox%` counts the rows waiting for the reader. Taking a row plays the `mailbox-took` moment.

Related: [Mailbox window](../menus/mailbox.md)
