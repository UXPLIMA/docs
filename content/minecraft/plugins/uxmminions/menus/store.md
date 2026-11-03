---
title: Store
order: 505
description: Where a player buys a minion, and why it ships closed.
icon: store
---

`menus/store.conf`: five rows. One line per type, drawn with its icon.

| Slot | Item | Click |
|---|---|---|
| 10 to 16, 19 to 25 | Each type | Buys one for 5000 when the player holds at least 5000 |
| 36 | Arrow, back | Your minions |

```hocon
open-requirement = ["minions:permission:uxmminions.store.buy"]
```

Nobody opens it without `uxmminions.store.buy`, which no player holds until you give it: grant it once you have decided
what a minion costs on your server. Until then the price in the file is a number somebody else chose.

| Action | Does |
|---|---|
| `minions:buy` | Hands one minion of that type over, free |
| `minions:buy:<n>` | Charges `n` in `economy.currency` and hands it over only if the whole `n` was taken |

The price lives in `minions:buy:<n>` itself: a charge written as a separate verb could not stop the hand over when the
player cannot pay. The `requirements` line only makes the refusal readable. There is one price for every type; a
different price per type is its own tile with its own slot and amount. `minions:money:<op> <amount>` reads a balance and
takes nothing, so it is safe in `view`, `requirements` and `open-requirement`.

Related: [Your minions](my-minions.md)
