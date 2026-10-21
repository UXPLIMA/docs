---
title: Currencies and gamepasses
order: 8
description: Coins, Gems, custom currencies, the seven egg gamepasses, and slot passes.
icon: coins
---

## The main currency

```lua
Currency = { Key = "Coins", Start = 100000 },
```

| Field | Meaning |
|---|---|
| `Key` | The currency name, used everywhere else |
| `Start` | What a new player begins with |

`Start = 100000` is a testing value. Set it to whatever a new player should actually have.

## Extra currencies

```lua
Currencies = {
    Gems = { Start = 500 },
},
```

Add as many as you like. They appear in the Roblox leaderboard automatically, and an egg
can cost one:

```lua
price = { currency = "Gems", amount = 50 },
```

## The currency display

```lua
CurrencyUI = {
    Enabled = true,
    Show = { "Coins", "Gems" },
},
```

The boxes in the corner, one per listed currency, in the order listed.

Every name in `Show` needs an icon in `PriceIcons`, or its box has no picture.

```lua
PriceIcons = {
    Coins = "rbxassetid://91195358993801",
    Gems = "rbxassetid://103825372958185",
    Robux = "rbxassetid://130480283103325",
},
```

`Robux` is special: it is used for any egg marked `robux = true`, whatever that egg's
fallback currency is.

## The egg gamepasses

```lua
Gamepasses = {
    Open3x = 864196470,
    Open8x = 864284268,
    AutoOpen = 864177554,
    FastOpen = 864200461,
    Luck1 = 864392167,
    Luck2 = 864150553,
    Luck3 = 864101978,
},
```

| Pass | Gives |
|---|---|
| `Open3x` | The open-three button |
| `Open8x` | The open-eight button |
| `AutoOpen` | Keeps opening automatically |
| `FastOpen` | Skips the open animation |
| `Luck1`, `Luck2`, `Luck3` | Luck tiers |

<Callout type="danger" title="Every one of these ids belongs to somebody else">

They are real, working gamepass ids from the original build. Replace all seven with your
own, or set any you do not want to `0`.

A player who happens to own the original creator's pass gets the benefit in your game for
free.

</Callout>

The multi-open buttons only appear on eggs whose `multiOpen` lists that count, so an egg
with `multiOpen = { 1 }` shows neither regardless of what a player owns.

## Luck

```lua
LuckValues = {
    Luck1 = 2,
    Luck2 = 3,
    Luck3 = 5,
},
```

A player gets the **highest** tier they own, not the sum. Owning all three is luck 5, not
luck 10.

Luck multiplies the effective weight of pool entries marked `luckBoost = 1`:

```
effectiveWeight = weight * (1 + (luck - 1) * luckBoost)
```

| Luck | Effect on a marked entry |
|---|---|
| 1 | None. This is the default |
| 2 | Twice the weight |
| 5 | Five times the weight |

Whether that is a large or a small change depends on the entry's share of the pool. A
0.05-weight Rainbow Dragon in a 145-weight pool goes from one in 2,900 to about one in 580
at luck 5, which is a real difference the player will feel.

`DefaultLuck = 1` is what a player with no pass has. Raising it globally makes the passes
worth less, so leave it at 1 and tune the tiers.

`LuckIcons` sets the picture on each luck button.

## Slot passes

```lua
Passes = {
    { gamePassId = 1875070251, target = "equip", amount = 3 },
    { gamePassId = 1875718256, target = "backpack", amount = 125 },
},
```

A list, so add as many as you want.

| Field | Meaning |
|---|---|
| `gamePassId` | Your gamepass id. `0` disables that line |
| `target` | `"equip"` or `"backpack"` |
| `amount` | How many slots it adds |

Owning several **adds them up**, unlike the luck tiers. Three equip passes of 3 give 9
extra slots.

That is what lets you sell a ladder: a small pass, a bigger pass, and a large pass, all
stacking.

<Callout type="danger" title="Both shipped ids are placeholders too">

Replace them. The plus button in the pets panel prompts players to buy whatever ids are in
this list.

</Callout>

## Robux eggs

An egg with `robux = true` and a `devProductId` is bought with Robux rather than currency.
The price shown comes live from the product. See [Eggs and hatching](eggs.md).

## Ownership caching

Pass ownership is checked once per player per session and cached. A purchase made in-game is
picked up immediately through the purchase-finished event, which recomputes the caps and
tells the client.

A pass bought on the website while the player is already in the game is not seen until they
rejoin.

## Plugging in your own economy

Currency goes through an adapter with three functions:

```lua
GetBalance(player, currencyKey)
Add(player, currencyKey, amount, reason)
Spend(player, currencyKey, amount, reason)
```

The default keeps balances on the player's `leaderstats`. If your game has its own economy,
replace the implementation so pets share one wallet with everything else.

The `reason` argument is passed through, so your economy can log that money came from
`breakable` or went to `egg:Pet1`.

## Pricing

The shipped prices are a demo:

| Egg | Price |
|---|---|
| Basic Egg | 2,500 Coins |
| Robux Egg | A developer product, 500 Coin fallback |
| Mythic Egg | 50 Gems |
| Huge Egg | 1 Coin |

The Huge Egg at one coin for a guaranteed Huge Elephant is a test entry, not a product.
Price it, or remove it, before you publish.
