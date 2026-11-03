---
title: Player shops
order: 407
description: Opening a shop, stock in the database, the claim check, limits, staff and server shops.
icon: store
---

A container in the world is the handle a visitor clicks. **The stock is a number in the database and never the
contents of the container.** A chunk that does not load, an explosion, a hopper or another plugin removing the block
costs no item, and the stock may hold more than the container could.

## Opening a shop

1. Open a container you are allowed to open, and look at it.
2. `/myshop open` opens a shop behind it.
3. `/myshop add <buy> <sell> <match>` adds the item in your hand as an offer.
4. `/myshop stock <offer> <amount>` moves items into an offer; `/myshop take` moves them back.
5. `/myshop price`, `remove`, `list`, `find` and `profits` manage and read the rest.

A visitor right clicks the container and the shop opens instead. Sneaking while clicking opens the container itself, so
an owner reaches their own chest. Breaking the container is refused for everybody but the owner, who closes the shop
with `/myshop close` and gets the stock back. From the [shop window](../menus/player-shops.md#a-shop) an owner turns
selling and buying off and chooses whether they are told about a trade.

## The claim check

A shop opens only behind a container the player has just opened. A claim plugin that refused the interaction has
already answered, so no claim plugin hook is needed. `player-shops.container-memory` (`30s`) is how long a container
stays open to becoming a shop.

## Limits

| Key | Default | Meaning |
|---|---|---|
| `limit` | 3 | Shops a player may own without a numbered node |
| `limit-permission` | `uxmshop.shops` | The prefix: `uxmshop.shops.10` is ten, `uxmshop.shops.*` no limit |
| `offers` | 27 | Offers one shop may hold |
| `tax.percent` | 0 | Taken out of what the receiving side is paid, in both directions |

A shop an operator opened with `/uxmshop adminshop` counts against nobody.

## Currency

A player shop prices in `shop.default-currency`, which must be able to pay an owner who is away. An item or experience
currency lives in the player, so it cannot; a currency with `pays-offline = false` is refused too. The refusal comes
where the currency is chosen, and says why.

## Staff

`/myshop staff add <player>` lets another player restock and reprice. Staff never delete the shop and never take the
money. `/myshop staff remove <player>` takes them off.

## Server shops

`/uxmshop adminshop`, while looking at a container, opens a shop with the server behind it. It never runs out and never
fills up, and only the visitor's balance moves. It names no owner and is drawn with `hologram.admin-lines`.

## What is not guaranteed

Two buyers of the last item in one offer, clicking in the same instant, can both pass the stock check; the overshoot is
bounded by how many click at once. A trade that fails at any step moves nothing.

Related: [Player shop commands](../commands/myshop.md), [The shop hologram](hologram.md)
