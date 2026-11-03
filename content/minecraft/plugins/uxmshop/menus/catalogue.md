---
title: Catalogue windows
order: 502
description: The hub, a section, the quantity window and the sell window.
icon: shopping-cart
---

## Hub

`/shop`. Sections in slots 10 to 16 (`shop:sections`), sorted by `order`; a click opens one (`shop:open`). The ender
chest at 22 runs `myshop list`. Paging at 18 and 26.

## Section

Lines in 10 to 16, 19 to 25 and 28 to 34 (`shop:lines`). Left click asks how many to buy (`shop:buy`), right click how
many to sell (`shop:sell`). The feather at 49 goes back (`shop:hub`); paging at 45 and 53. A shortcut line follows to its
section (`shop:open`).

| Fact a tile may name | Shows |
|---|---|
| `price-buy`, `price-sell` | What one costs and pays |
| `stock` | How many are left, when the line counts |
| `reset` | How long until an allowance comes back |
| `match` | How strictly the line matches |
| `state` | Which way it trades, and whether any is left |

## Quantity

Keeps a player out of chat: the amount is chosen on buttons.

| Slot | Button |
|---|---|
| 10, 11, 12 | Less 64, 8, 1 (`shop:less:<n>`) |
| 13 | The amount and total; left click confirms a catalogue trade (`shop:confirm`), right click a player shop trade (`shop:offer-confirm`) |
| 14, 15, 16 | More 1, 8, 64 (`shop:more:<n>`) |
| 22 | Back (`shop:back`) |

An action that takes a number carries it after a colon, as `shop:more:8`. Facts: `amount`, `total`, `price-buy`,
`price-sell`.

## Sell

`/sell window`. No buttons: it opens empty, and what is left in it when it closes is sold. What cannot be sold comes
straight back, and the player is told how many did. The file sets its size and title.

Related: [Selling](../features/selling.md)
