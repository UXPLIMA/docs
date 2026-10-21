---
title: Bail
order: 5
description: Cash bail, Robux bail, the BailId link, and plugging in your own economy.
icon: banknote
---

Bail lets a jailed player end their sentence early, with in-game currency, with Robux, or
both.

Both are per team pair, so a Police arrest and a Federal arrest can cost different amounts.

## Cash bail

```lua
CashBail = true,
CashBailFee = 400,
CashBailText = "400$ BAIL",
CashBailCurrency = "Cash",
```

| Field | Meaning |
|---|---|
| `CashBail` | Show the button |
| `CashBailFee` | The price |
| `CashBailText` | The button label, written by you |
| `CashBailCurrency` | The `leaderstats` value to charge |

`CashBailCurrency` must match the name of your money stat exactly. The default is `Cash`.

<Callout type="warning" title="The label and the fee are two separate values">

`CashBailText` is free text. Changing `CashBailFee` to 250 without changing the label leaves
a button that says `400$ BAIL` and charges 250.

Change both, every time.

</Callout>

## Robux bail

```lua
RobuxBail = true,
RobuxBailFee = 20,
RobuxBailId = 3252720675,
RobuxBailText = "$Price R$ BAIL",
```

`RobuxBailId` is a **developer product** id, not a gamepass. The button stays hidden while
it is `0`.

`$Price` in the label is replaced with the product's live price, fetched from Roblox and
cached. Until the fetch returns, the button reads `...`. `RobuxBailFee` is reference only
and is not used for anything.

<Callout type="danger" title="The shipped product id is not yours">

```lua
RobuxBailId = 3252720675,
```

Replace it with your own developer product id, or set it to `0`. Leaving it prompts your
players to buy somebody else's product, and the purchase will not release them.

</Callout>

## The BailId link

By the time a player is jailed their team has changed, so the system cannot find their team
pair by team name any more. It records the pair's `BailId` on the profile at arrest time and
looks the pair back up by that number.

```lua
BailId = 1000,
```

| Rule | |
|---|---|
| Unique per team pair | Two pairs sharing a number get each other's prices |
| At 1000 or above | The free-for-all entry uses 900 |
| Never reused after launch | An old sentence carrying a retired BailId finds no bail |

The last one matters if you restructure teams: a player who was jailed under the old numbers
and comes back after your update simply gets no bail buttons and serves their time.

## The buttons

`ArrestInfoFrame.BailFrame` holds two children:

| Instance | |
|---|---|
| `CashBailFrame` | With a `ClickButton` and a `LabelText` |
| `RobuxBailFrame` | Same |

Whichever are enabled are shown and repositioned: one bail option centres itself, two sit
left and right. The whole `BailFrame` hides when neither is available.

## Buying out

| Step | Cash | Robux |
|---|---|---|
| 1 | The server checks they are still jailed | The player is prompted to buy |
| 2 | It checks the balance | Roblox processes the purchase |
| 3 | It deducts the fee | The receipt arrives |
| 4 | It releases them | It releases them |

Both paths run the same release: teams restored, profile cleared, character respawned.

A Robux receipt for a player who is no longer jailed is still granted, because refusing it
would mean charging them and giving nothing. They pay for nothing, which is why the button
is hidden the moment the sentence ends.

## Refusals

| Reason | Means |
|---|---|
| `NotJailed` | The sentence already ended |
| `NoBail` | That BailId has no cash bail configured |
| `Insufficient` | Not enough currency |

## Plugging in your own economy

Cash bail goes through an adapter. The default reads and writes a `leaderstats` value:

```lua
GetBalance(player, keyName)
CanAfford(player, keyName, amount)
Spend(player, keyName, amount, reason)
```

If your game keeps money in a profile store rather than in leaderstats, replace the
implementation with your own three functions:

```lua
local EconomyAdapter = require(path.to.EconomyAdapter)

EconomyAdapter.set({
    GetBalance = function(player, key) return MyEconomy:Get(player, key) end,
    CanAfford = function(player, key, amount) return MyEconomy:Get(player, key) >= amount end,
    Spend = function(player, key, amount, reason) return MyEconomy:Take(player, key, amount, reason) end,
})
```

Call it from a server script before anybody is jailed. The `reason` argument is passed
through so your economy can log what the money was spent on.

## The receipt handler

Robux bail installs `MarketplaceService.ProcessReceipt`. Products that are not bail products
return `NotProcessedYet`, so Roblox retries them and another system's handler can take them.

<Callout type="warning" title="Only one script may own ProcessReceipt">

Roblox allows a single `ProcessReceipt` callback per server. If another system in your game
also sets one, whichever runs last wins and the other's purchases stop working.

Route every developer product in your game through one handler, and have it call this
system's bail handler for ids it does not recognise.

</Callout>

## Turning bail off

Set `CashBail = false` and `RobuxBailId = 0` on every team pair. The `BailFrame` then never
appears, and sentences are served in full.
