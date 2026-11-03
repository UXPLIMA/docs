---
title: Selling
order: 401
description: Fixed-price listings, retail stacks, durations, the price band, repricing, cancelling and how a seller is paid.
icon: tag
---

## Listing

`/auction sell <price> [amount] [-t time] [-c currency] [-r]` lists the item in hand. Without a price it opens the
[sell window](../menus/sell.md), where the price, the amount, the time, the currency, the kind and retail are clicked.

| Rule | Set by |
|---|---|
| How long it stands | `-t`, or `house.listing.default-duration` (2d); more than `maximum-duration` (7d) is refused |
| The currency | `-c`, or the currency with the lowest `order` |
| The price band | `house.price.minimum.<currency>`, `maximum.<currency>`; an unnamed currency has none |
| How many one seller may list | `house.max-listings-per-seller`; 0 is no limit |
| The listing fee | `fees.listing`, taken when the listing is written and not given back on expiry |

A ceiling is worth setting: it catches a seller who typed one zero too many.

## Retail

With `-r`, or retail on in the window, buyers may take part of the stack: four buyers may each take some of 64. Without
it, a buyer takes all of it. `house.allow-retail = false` turns retail off for everybody. An auction is never sold in
parts.

## Repricing and cancelling

`/auction reprice <listing> <price>` lowers the price of your own listing, for `fees.reprice`. `/auction cancel <id>`
takes it back while `house.allow-cancelling` is on; the item goes to your mailbox.

## Buying your own

`house.allow-buying-your-own = false` stops a seller buying or bidding on their own listing. A server that taxes a sale
can turn it on to let a seller take a listing back at a cost.

## Being paid

When a listing sells, the seller is paid straight into their balance if the currency can pay a player who is away
(`pays-offline`), and otherwise into their mailbox. `fees.sale-tax` is kept out of what they earn.

A new listing at or below an open order's price is offered to the best paying orders first and sells into them at once;
see [Buy orders](orders.md).

Related: [Sell window](../menus/sell.md), [Fees](fees.md)
