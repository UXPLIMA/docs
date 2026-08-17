---
title: The auction house
order: 10
description: Listing an owned car, bidding, settlement, and what the single server scope means.
icon: gavel
---

```lua
Auction = {
    Enabled = true,
    OpenKey = Enum.KeyCode.H,
    MinDurationSeconds = 60,
    MaxDurationSeconds = 3600,
    DefaultDurationSeconds = 300,
},
```

Press H to open the panel. A player lists a car they own with a starting price and a
duration, everybody in the server sees it, and the highest bid at the end wins.

## Listing

| Rule | |
|---|---|
| Must own the car | Issued duty vehicles cannot be listed |
| One listing per car per seller | A second attempt is refused |
| Starting price | Floored at zero |
| Duration | Clamped between `MinDurationSeconds` and `MaxDurationSeconds` |

The car stays in the seller's garage while it is listed. They can drive it, sell it back to
the dealership, or auction a second car at the same time.

## Bidding

| Rule | |
|---|---|
| The first bid | Must at least match the starting price |
| Later bids | Must beat the current bid by at least one |
| The seller | Cannot bid on their own listing |
| Affordability | Checked against the live balance when the bid is placed |

<Callout type="warning" title="Bids are not held in escrow">

Nothing is deducted when a bid is placed. The money is only taken at settlement, and the
balance is checked again then.

So a player can bid on three cars with enough money for one, or bid high and spend the
money before the timer runs out. In the second case the sale simply fails and nobody gets
anything.

If your economy is tight, keep durations short. Five minutes, the shipped default, leaves
little room for it.

</Callout>

## Settlement

The timer is checked every two seconds. When a listing expires:

| Condition | Outcome |
|---|---|
| A bidder, both players present, seller still owns it, bidder can pay | The car and the money change hands |
| Any of those fails | No sale. The seller is told, and keeps the car |

There is no partial state. The transfer happens as one step or not at all.

The seller receives the full winning bid. The system takes no cut, which is worth
considering: an auction house with no fee is a perfectly efficient way to move money
between players, and a small percentage fee is the usual way to drain some of it back out.

## Single server

<Callout type="danger" title="Auctions are per server and live in memory only">

Listings are held in one server's memory. Players in other servers cannot see or bid on
them. Nothing is written to a DataStore.

A server restart destroys every open listing with no notice to anybody, and a seller who
leaves has their listings cancelled immediately, mid-auction, even if somebody has bid.

</Callout>

That makes the feature best suited to games where players gather in one busy server. For a
game spread across many small servers, most listings will expire having been seen by
nobody.

## Buying a locked car second hand

An auction transfers ownership directly and does not consult the vehicle's `Permissions`.
A player who buys a gamepass-locked car at auction owns it outright.

Whether that is a hole depends on what the lock is for. If the lock exists to sell a
gamepass, keep those cars out of the auction house by disabling the feature or by giving
them a lock that also stops them being useful, such as pad restrictions the buyer cannot
reach.

Issued duty vehicles are safe: they are never owned, so they can never be listed.

## Turning it off

`Auction.Enabled = false` refuses listings and bids and stops the panel doing anything.
Nothing is stored, so there is nothing left behind.
