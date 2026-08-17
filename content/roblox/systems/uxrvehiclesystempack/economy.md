---
title: Money and earning
order: 4
description: Where the balance lives, earning while driving, multipliers and garage slots.
icon: coins
---

## Two places the balance can live

```lua
Currency = "$",
StartingBalance = 0,
Leaderstats = {
    Enabled = true,
    FolderName = "leaderstats",
    StatName = "Cash",
    ValueClass = "IntValue",
    CreateMissing = true,
    ClampNonNegative = true,
},
```

| Mode | Balance lives in |
|---|---|
| `Leaderstats.Enabled = false` | The system's own profile, saved to its DataStore |
| `Leaderstats.Enabled = true` | `leaderstats.Cash`, owned by your game |

Leaderstats mode is what you want when the game already has money: a jobs system, a
robbery system, a shop. Buying, selling, earning, refuelling, fines and admin edits all read
and write that one stat, and the UI mirrors changes made by your other systems live.

<Callout type="danger" title="Buying is broken while leaderstats mode is on">

The purchase path checks affordability against the internal profile balance instead of
going through the balance accessor. In leaderstats mode that internal number is never
written, so it stays at zero and every priced vehicle is refused as unaffordable.

Everything else in the system reads the balance correctly. It is only the buy check.

Set `Leaderstats.Enabled = false` until this is fixed, or keep the whole catalog free and
gate cars with `Permissions` instead of prices.

</Callout>

Two more places show the internal number rather than the live one: the balance sent when a
dealership opens, and the balance shown on the admin panel's target card. Both read zero in
leaderstats mode even though spending works.

### Starting money

`StartingBalance` is granted once, on the first join, and only when the system owns the
balance. In leaderstats mode the system never touches starting money, and never wipes it:
that is your game's business.

`StarterVehicleId` is granted on the first join in either mode.

## Earning while driving

```lua
Earn = {
    Enabled = true,
    MinSpeed = 30,
    RewardPerSecond = 5,
    SampleInterval = 1,
    MaxPerSecond = 50,
    RequireVehicleSeat = true,
    RoundReward = true,
},
```

Every second, each player is sampled. Above `MinSpeed` they earn `RewardPerSecond`, times
their multipliers.

| Field | Does |
|---|---|
| `MinSpeed` | Studs per second the player must be doing to earn |
| `RewardPerSecond` | The base payout per sample |
| `SampleInterval` | Seconds between samples |
| `MaxPerSecond` | A ceiling on the **base** payout only |
| `RequireVehicleSeat` | `true` earns only while seated in a `VehicleSeat` |
| `RoundReward` | Round the multiplied total to a whole number |

<Callout type="warning" title="MaxPerSecond does not cap the final payout">

It caps the base before multipliers. A base of 5 with Premium, two gamepasses and a group
bonus can pay far more than `MaxPerSecond`.

It is an anti-exploit floor under one number, not a ceiling on the economy. Work out your
worst case by hand: base times every multiplier a single player can hold at once.

</Callout>

### The multipliers

```lua
Multipliers = {
    Enabled = true,
    Premium = { Enabled = true, Multiplier = 2 },
    Gamepasses = { },
    Groups = { },
    Teams = { },
},
```

The formula is:

```
base x Premium x (each owned gamepass) x (1 + sum of group and team bonuses)
```

| Source | Written as | Effect |
|---|---|---|
| Premium | `Multiplier = 2` | Doubles |
| Gamepasses | `[123456789] = 2` | Multiplies, and they compound |
| Groups | `[987654321] = 0.10` | Adds ten percent |
| Teams | `["Civilian"] = 0.05` | Adds five percent |

Gamepasses multiply and stack, so two 2x passes give 4x. Group and team bonuses are added
together first and applied once, so they behave much more gently. That difference is
deliberate: sell the multiplicative ones, give away the additive ones.

Ownership is checked once per player per pass and cached for the session, and a pass bought
mid-session is picked up immediately.

### Passengers

```lua
Passengers = { Enabled = false, Percent = 0.5 },
```

Off by default. On, every other player seated in the same vehicle model earns that fraction
of the driver's reward. The driver loses nothing: it is created money, not shared money.

### When earning stops

| Situation | Why |
|---|---|
| Below `MinSpeed` | Nothing to reward |
| Not in a `VehicleSeat` | With `RequireVehicleSeat = true` |
| Out of fuel | Only when `Fuel.EmptyMode` is `"warn"` |
| On a test drive | Test drives never earn |

The test drive rule matters: without it, an endless loan of the fastest car in the catalog
would be a better income than owning anything.

## Garage slots

```lua
Garage = {
    Enabled = true,
    BaseSlots = 5,
    SlotGamepasses = {},
},
```

A cap on how many vehicles a player may own at once. The counter is shown in the panel's
top bar, and a purchase over the cap is refused with a message telling them to sell
something or buy more slots.

```lua
SlotGamepasses = { { GamepassId = 111, Slots = 5 }, { GamepassId = 222, Slots = 10 } },
```

Each pass the player owns adds its slots. They stack.

<Callout type="info" title="Issued cars are free of the cap">

Auto-granted duty vehicles are not written to the profile, so they never take a slot. A
police officer with five slots still has five slots for their own cars.

</Callout>

`Garage.Enabled = false` removes the cap and hides the counter.

## The currency symbol

```lua
Currency = "$",
```

Prepended to every amount the system prints. It is a string, so a coin emoji, a word or a
Robux glyph all work.

It is cosmetic and never parsed, so changing it later is safe.
