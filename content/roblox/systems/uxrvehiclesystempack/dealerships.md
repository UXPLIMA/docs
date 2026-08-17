---
title: Dealerships
order: 3
description: Storefronts, who may open them, what they stock, and the sale prices.
icon: store
---

A dealership is a storefront: a title, a way to open it, an optional access gate and a
stock list. You can have as many as you like.

```lua
Dealerships = {
    {
        Id = "general",
        Title = "Vehicle Dealership",
        General = true,
        OpenKey = Enum.KeyCode.G,
        PromptPartName = "DealershipZone",
    },
    {
        Id = "civilian",
        Title = "Civilian Dealership",
        PromptPartName = "CivilianDealer",
    },
    {
        Id = "police",
        Title = "Police Dealership",
        PromptPartName = "PoliceDealer",
        Access = {
            Teams = { "Police" },
            GroupId = 0,
            GroupMinRank = 1,
            GamepassId = 0,
            OwnerOnly = false,
        },
    },
},
```

| Field | Does |
|---|---|
| `Id` | The internal name. Referenced by `VehicleData.Dealerships` |
| `Title` | The panel heading, and the text on the proximity prompt |
| `General` | Marks the fallback storefront. Exactly one should have it |
| `OpenKey` | A keyboard shortcut that opens this storefront anywhere |
| `PromptPartName` | A part in the world folder that gets an E prompt |
| `Access` | Who may open it |

## Opening one

Three ways in:

| Route | Applies to |
|---|---|
| The `OpenKey` | Any dealership that sets one. Ships as G on the general storefront |
| A proximity prompt | Any dealership with a `PromptPartName` |
| The on-screen button | Opens the general storefront |

The prompt is created on any part in the world folder whose name matches, including parts
added after the server starts, so an NPC that spawns later still gets its prompt.

<Callout type="info" title="One prompt per part">

A part that already has a `ProximityPrompt` is skipped. If you want your own prompt styling,
add the prompt yourself and the system leaves it alone, but nothing will open the panel: the
handler is attached to the prompt it creates.

Restyle the created prompt instead, or use a differently named part.

</Callout>

## Who may open it

`Access` gates the whole storefront. Every configured dimension must pass.

| Field | Passes when |
|---|---|
| `Teams` | The player is on one of the named teams |
| `GroupId` and `GroupMinRank` | They are in the group at that rank or above |
| `GamepassId` | They own the pass |
| `OwnerOnly` | They are the game creator, or rank 255 in the owning group |

`0` and `false` mean "skip this check". A dealership with no `Access` table is open to
everybody.

<Callout type="tip" title="Two entries make an OR">

There is no OR inside one `Access` table. To let either police or medics into the same
showroom, define two dealership entries pointing at the same `PromptPartName`, one gated on
each team.

Both prompts are created on the same part; only the one the player passes will open.

</Callout>

## What each one stocks

A vehicle appears in a dealership when its `Dealerships` list names that dealership's `Id`.
A vehicle with no `Dealerships` list appears only in the general storefront.

| Vehicle row | Sold in |
|---|---|
| `Dealerships = { "general", "civilian" }` | Both of those |
| `Dealerships = { "police" }` | The police storefront only |
| No `Dealerships` field | The general storefront only |

The stock check runs on the server for both the listing and the purchase, so a client that
asks to buy a police car from the civilian storefront is refused.

## The panel

| Area | Shows |
|---|---|
| Top bar | Balance, garage slot counter, close |
| Left | The vehicle list, search, filters, sort, an owned-only toggle |
| Centre | The 3D preview, the purchase button, the test drive button |
| Right | Name, category, description, price, the stat sheet, swatches, the plate button |

Filters cover Type, Brand, Sort and Owned. Sort offers price low to high, price high to
low, name and speed.

## Discounts

Three sources, and the biggest one wins. They do not stack.

| Source | Config | Applies to |
|---|---|---|
| VIP | `Vip.GamepassId`, `DiscountPercent` | Everything, for pass owners |
| Daily deal | `DailyDeal.DiscountPercent` | One car a day, the same for everybody |
| Flash sale | `FlashSale.DiscountPercent` | A rotating car, for a window |

```lua
Vip = { GamepassId = 0, DiscountPercent = 10 },
DailyDeal = { Enabled = true, DiscountPercent = 25 },
FlashSale = { Enabled = true, DiscountPercent = 40, IntervalSeconds = 1800, DurationSeconds = 300 },
```

### How the rotation is picked

Both rotations are derived from the clock rather than stored, so every server in your game
agrees without talking to each other.

| | |
|---|---|
| Daily deal | The day number since the epoch, modulo the number of eligible cars |
| Flash sale | The current interval window, modulo the same list |

The eligible list is every car with `OnSale = true` and a price above zero, sorted by id.

<Callout type="warning" title="Adding a car reshuffles both rotations">

The list is indexed by position, not by id. Adding or removing a catalog row shifts
everything after it, so today's deal changes the moment you publish.

Harmless day to day. Worth knowing if you have advertised tomorrow's deal.

</Callout>

With the shipped 1800 and 300, a flash sale runs for five minutes at the top of every half
hour, on a different car each time. The countdown is shown on the card.

## Trade-in

```lua
TradeIn = { Enabled = true, RefundPercent = 50 },
```

Selling an owned car back returns that percentage of its **catalog** price, rounded. Not of
what the player paid: a car bought in a 40 percent flash sale and sold back at 50 percent
returns more than it cost.

<Callout type="warning" title="Discounts plus trade-in can be a money printer">

Buy at 60 percent during a flash sale, sell back at 50 percent, and each cycle loses 10
percent. That is fine. Raise `RefundPercent` to 70, or stack a VIP discount on top of a
deal, and the loop turns a profit for the player.

Keep `RefundPercent` comfortably below `100` minus your deepest discount.

</Callout>

Selling frees the garage slot. The saved paint, plate, fuel level and trunk contents stay
on the profile under that vehicle id, so buying the same car again restores all of it.
