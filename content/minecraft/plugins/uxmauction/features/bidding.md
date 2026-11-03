---
title: Bidding
order: 402
description: Auctions, the bid step, anti-sniping, refunds and winning.
icon: trending-up
---

`/auction auction <price> [-t time] [-c currency]` lists the item in hand for bids, starting at the price. In the sell
window, the kind button switches between a fixed price and an auction.

## A bid

A bid is in the currency of the listing; a bidder never names one. It must be at least the leading bid plus the step of
that currency, `house.bidding.step.<currency>`; the shipped step for `coins` is `1`. The money of a bid is held while it
leads.

The bid button in the [listing window](../menus/listing.md) offers the next step, worked out by the house.

| Moment | Effect |
|---|---|
| `bid-leading` | Your bid was taken and you lead |
| `bid-outbid` | Your bid was taken but somebody else was already higher |
| `bid-beaten` | Somebody bid over you; your money comes back to your mailbox |
| `auction-sold` | Your auction ended and sold |
| `auction-won` | An auction you bid on ended and you won it; the item goes to your mailbox |

A step of zero takes any offer that is not lower, which turns an auction into a war of one coin per click.

## Anti-sniping

A bid in the last `house.bidding.anti-snipe.window` (30s) of an auction adds `extension` (1m) to it, so the winner is
whoever offered the most and not whoever was awake at the last second. A window of `0s` turns it off.

## Ending

An auction that ends with bids goes to the highest bidder; one that ends without goes back to the seller's mailbox. An
operator who cancels an auction with bids gives the money back before the item.

Related: [Listing window](../menus/listing.md), [The mailbox](mailbox.md)
