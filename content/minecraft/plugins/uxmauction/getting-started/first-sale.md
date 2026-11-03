---
title: Your first sale
order: 104
description: List an item, buy it with another player, and collect the money.
icon: flag
---

1. As a seller, hold a diamond sword and run `/auction sell 500`. The sword is on the shelf for two days at 500 coins.
2. As a buyer, run `/auction` (or `/ah`). The house opens on the newest listings. Click the sword, then the emerald
   block: you pay 500 and the sword is in your inventory.
3. The seller is paid at once, into their balance. With a currency that cannot pay a player who is away, the money waits
   in their mailbox: `/auction mailbox`.

Next:

- `/auction auction 100 -t 1h` puts the item in hand up for bids, starting at 100, for an hour.
- `/auction order 20 64 -m material` asks the house to buy 64 of the item in hand at 20 each.
- `/auction` with no price opens the [sell window](../menus/sell.md) to set everything with clicks.

Related: [Selling](../features/selling.md), [Player commands](../commands/player.md)
