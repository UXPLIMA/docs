---
title: Skip the wait
order: 5
description: The Developer Product that clears the cooldown, and sharing ProcessReceipt.
icon: coins
---

Players can pay Robux to make the next reward claimable now. It clears the timer and
nothing else: no day is skipped, no reward is lost, the streak is untouched.

## Creating the product

1. On the Creator Dashboard, open your experience, then **Monetization, Developer
   Products**, and create one. Name it something like "Skip Daily Timer" and set a price.
2. Copy its id.
3. In `Shared/Config/Settings.luau`:

```lua
Skip = {
    Enabled = true,
    DevProductId = 1234567890,
    PriceText = "99 R$",
    ManageProcessReceipt = true,
},
```

`PriceText` is only a label. Change the real price on the dashboard and update this
string to match, or the button will advertise the wrong number.

The button appears only when `Enabled` is `true` and `DevProductId` is above zero.

## What the purchase does

The receipt handler sets the player's last claim time to one full interval ago. The next
resolve therefore reports the reward as claimable, with the same day and the same streak
index as before.

Applying it twice does nothing extra, which is why a retried receipt is safe to grant
again. Roblox retries `ProcessReceipt` until it is told the purchase was handled, and an
effect that is not idempotent gets applied several times for one payment.

## Sharing ProcessReceipt

`MarketplaceService.ProcessReceipt` is one global callback and the last script to set it
wins. If this is the only thing in your game selling Developer Products, leave
`ManageProcessReceipt = true` and stop reading.

If anything else sells one, set it to `false` and route receipts yourself:

```lua
local RewardService = require(
    game.ServerScriptService.uxrDailyRewardsSystem.DailyRewards.RewardService
)

MarketplaceService.ProcessReceipt = function(receiptInfo)
    local decision = RewardService.handleReceipt(receiptInfo)
    if decision ~= nil then
        return decision
    end

    -- your own products from here
    return myOwnHandler(receiptInfo)
end
```

`handleReceipt` returns `nil` for any product that is not the skip product, which is the
signal to keep processing. It returns `PurchaseGranted` when it recognised and applied
the purchase.

<Callout type="danger" title="Getting ProcessReceipt wrong charges players for nothing">

With two systems both claiming `ProcessReceipt`, one of them silently loses. Its products
are charged by Roblox and never delivered, and there is no error in Output because
nothing failed: the callback simply belongs to somebody else.

Exactly one owner. Everything else routes through it.

</Callout>

## Not returning a decision

The shipped handler refuses to guess about products it does not own. When it manages the
callback and sees an unknown product, it does not return `PurchaseGranted`, because
granting somebody else's purchase without running their code takes the player's Robux and
delivers nothing. Roblox keeps retrying, which is recoverable; a wrongly granted receipt
is not.

That is the case `ManageProcessReceipt = false` exists to fix.

## Testing it

Developer Product purchases work in Studio for the place owner without spending Robux.
Set `ClaimIntervalSeconds` low, claim once, then buy the skip while the countdown is
running: the panel should switch to claimable immediately and `Hooks.onSkipPurchase`
should fire.
