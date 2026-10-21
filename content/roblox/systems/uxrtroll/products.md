---
title: Products and credits
order: 4
description: Fourteen developer products, live prices, banked purchases and sharing ProcessReceipt.
icon: shopping-cart
---

## Fourteen products

Two per troll: one for the single target button, one for the everyone button.

```lua
OneProductId = 3707848824,
AllProductId = 3707849001,
```

A product id of `0` hides that button. A troll with `Enabled = false` hides both.

## Prices are never in config

There is no price field anywhere in this system, deliberately. A number written in a config
file goes stale the first time you change the price on the website, and then the button lies
to your players.

Instead the button asks Roblox for the current price and shows that.

```lua
Format = utf8.char(0xE002) .. "%d",
```

`%d` is the price in Robux. `utf8.char(0xE002)` is the Robux symbol: Roblox's own fonts draw
that code point as the icon, and it is written this way rather than pasted in so the config
file stays plain ASCII.

Drop it for a bare number.

## Credits

```lua
Free = "FREE (%d)",
```

A player can end up owning a paid troll they have not used. The button then reads `FREE`
with a count, and pressing it spends the credit instead of opening a purchase prompt.

Credits happen when:

| Situation | |
|---|---|
| The target left before the receipt arrived | The buyer is told privately, and the go is banked |
| The target became immune | Same |
| No target was selected | Same |
| The receipt arrived with no recorded intent | The go is banked |

That last one is the important safety net: if a player buys a troll and their client
forgets which target it was for, perhaps because they rejoined between paying and the
receipt landing, they get a credit rather than nothing.

<Callout type="info" title="Credits are saved with the player's profile">

They persist across sessions, so a purchase made just before a disconnect is still waiting
when the player comes back.

</Callout>

Credits are per troll and per scope, so a banked Kill on one target does not become a
banked Kill on everybody.

## Duplicate receipts

Roblox can deliver the same receipt more than once. The system records the last fifty
purchase ids on the player's profile and grants each one only once.

<Callout type="warning" title="The guard needs a working profile">

If the profile service is unavailable, a warning is logged and duplicate protection is off:
a receipt Roblox delivers twice would be granted twice.

That is a startup problem rather than a normal condition, but the warning in Output is worth
paying attention to.

</Callout>

## Sharing ProcessReceipt

```lua
ManageProcessReceipt = true,
```

Roblox allows exactly one `ProcessReceipt` callback per game. Whichever script sets it last
wins, and every other product in your game stops working.

If this is the only thing selling products, leave it on.

If you sell anything else, set it to `false` and call the handler from your own:

```lua
local PurchaseService = -- the system's PurchaseService

MarketplaceService.ProcessReceipt = function(receiptInfo)
    local decision = PurchaseService:HandleReceipt(receiptInfo)
    if decision ~= Enum.ProductPurchaseDecision.NotProcessedYet then
        return decision
    end

    -- your own products here

    return Enum.ProductPurchaseDecision.NotProcessedYet
end
```

`HandleReceipt` returns `NotProcessedYet` for anything that is not one of this system's
products, which is your signal to carry on.

## What a failed grant means

| Return | Meaning |
|---|---|
| `PurchaseGranted` | The troll landed, or was banked as a credit |
| `NotProcessedYet` | Nothing happened yet. Roblox will retry |

An everyone troll that reached nobody returns `NotProcessedYet`, so the purchase is retried
rather than lost. Returning `PurchaseGranted` for a troll that did nothing would be taking
money for nothing.

## Pricing advice

The two scopes are very different products and should not be priced the same.

| Button | Suggestion |
|---|---|
| One target | Cheap enough to be an impulse. This is your volume |
| Everyone | Several times the single price. This is your event |

Kick on the everyone button empties your server, so either price it as the most expensive
thing you sell or leave its `AllProductId` at `0`.

The server cooldown limits how often everyone trolls can fire regardless of price, so
pricing and `AllCooldown` work together. See [Targeting and immunity](targeting.md).
