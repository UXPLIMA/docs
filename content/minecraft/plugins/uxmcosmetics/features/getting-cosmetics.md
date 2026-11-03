---
title: Getting cosmetics
order: 302
description: The four ways a player gets a piece, prices, purchase safety, and permission nodes.
icon: wallet
---

| Way | How |
|---|---|
| Buying | A piece with `price` above 0 is for sale in its family window |
| Mystery boxes | A box draws one piece from its table; see [Mystery boxes](mystery-boxes.md) |
| Staff | `/uxmcosmetics grant <player> <cosmetic>`, online or away |
| A node | `uxmcosmetics.<family>.<id>`, or the `permission` the piece names |

## Buying

The price is taken in `economy.currency`, then ownership is written. If the write fails the money is put back before
the failure is reported, so a player never pays for nothing. A player who already owns the piece, or holds its node,
is not charged.

| Answer | When |
|---|---|
| `buy.done` | Bought |
| `buy.already-owned` | Already theirs |
| `buy.not-for-sale` | `price = 0` |
| `buy.disabled` | `economy.purchase-enabled = false` |
| `buy.no-economy` | The currency's plugin is not on the server |
| `buy.not-enough` | With the price and the balance |

`economy.purchase-enabled = false` turns buying off; the wardrobe still opens and wears.

## Prices

`price` in the piece's entry, in `economy.currency`, written with `economy.format`. The shipped prices run from 250
for a common trail to 5500 for a legendary mount; see the [Catalogue](../catalogue/index.md).

## Nodes

Every piece has a node: `uxmcosmetics.<family>.<id>` unless its entry writes `permission`. A player who holds it has
the piece without owning it, and it shows as **Yours**.

| Node | Gives |
|---|---|
| `uxmcosmetics.trail.flame_trail` | One piece |
| `uxmcosmetics.*` | Every piece and every box, while `permissions.wildcard = true`. Default `op` |

`permissions.wildcard = false` makes each piece need its own node; a server that grants pieces by rank turns it off.
It needs a restart.

## Revoking

`/uxmcosmetics revoke` takes ownership away and takes the piece off. A piece held by node is removed by the permission
plugin.

Related: [Mystery boxes](mystery-boxes.md), [Permissions](../permissions/reference.md), [Economies](../integrations/economies.md)
