---
title: Fuel and charging
order: 7
description: Draining, gas stations, chargers, and what happens at empty.
icon: fuel
---

```lua
Fuel = {
    Enabled = true,
    Capacity = 60,
    DrainPerSecond = 0.4,
    MinSpeed = 20,
    SampleInterval = 1,
    PricePerLiter = 17,
    PricePerKWh = 0.65,
    EmptyMode = "warn",
    GasStationName = "GasStation",
    ChargerName = "Charger",
},
```

Fuel is per player per vehicle and saved with the profile, so a car left half empty is
still half empty tomorrow.

## Draining

Fuel drops only while all three are true: the player is seated in the active vehicle, the
vehicle is above `MinSpeed`, and there is fuel left.

With the shipped numbers, a full 60 unit tank lasts 150 seconds of continuous driving.
That is deliberately short for testing. For a roleplay game, something like `0.05` per
second gives twenty minutes of driving, which is closer to a chore than a punishment.

<Callout type="tip" title="Pick the drain rate from how long a session is">

Work backwards. Decide how often you want a player at the pump, multiply by 60 seconds,
and divide `Capacity` by that.

Ten minutes between fills at a 60 unit tank is `60 / 600`, so `DrainPerSecond = 0.1`.

</Callout>

## Petrol and electric

A catalog row with `IsElectric = true` charges instead of refuels. The difference is
cosmetic and financial rather than mechanical:

| | Petrol | Electric |
|---|---|---|
| Station | `GasStation` | `Charger` |
| Price | `PricePerLiter` | `PricePerKWh` |
| Unit shown | L | kWh |
| Gauge label | FUEL | CHARGE |

Using the wrong station is refused with a message telling the player which one they need.
At the shipped prices, a full tank costs 1020 and a full charge costs 39, which is a
straightforward way to make electric cars cheaper to run than petrol ones.

## Stations

Any `BasePart` under the world folder named `GasStation` or `Charger` gets an E prompt.
Parts added after the server starts are picked up too.

A part that already has a `ProximityPrompt` is skipped, so add your own only if you also
want your own handling.

<Callout type="warning" title="The prompt is created on the client">

Stations, unlike trunk prompts, are wired up by the client. Each player creates their own
prompt on the same part.

That is invisible in play, but it means a station only exists for players whose UI loaded,
and the refuel request itself is still validated on the server.

</Callout>

### Refuelling

| Check | Result |
|---|---|
| No active vehicle | "Spawn a vehicle first" |
| Not seated in it | "Get in your vehicle to refuel" |
| Already full | "Already full" |
| Wrong station type | Told which one to use |
| No money at all | "Not enough money to refuel" |

The panel lets the player choose how much to buy, and the amount is clamped between what
they have now and the tank size. If they cannot afford the full amount they get as much as
their balance covers rather than being refused, so a player is never stranded with money in
their pocket.

## At empty

```lua
EmptyMode = "warn",
```

| Mode | At zero |
|---|---|
| `"warn"` | A toast, and earning while driving stops |
| `"stop"` | The seat's `MaxSpeed` is set to zero, so the car cannot pull away |
| `"cosmetic"` | Nothing but the gauge |

`"stop"` remembers the seat's original `MaxSpeed` and restores it on refuel, so it works
with any chassis whose top speed comes from the seat.

<Callout type="warning" title="Stop mode strands players">

A car that runs dry in the middle of nowhere cannot be driven to a station. The player's
way out is to despawn it and spawn it again, which costs them nothing, so all `"stop"`
really achieves is an interruption.

`"warn"` is the better default for most games: the player keeps driving but stops earning,
which is a reason to visit the pump rather than a wall.

</Callout>

## The gauge

The HUD gauge is a frame named `FuelGauge` on the system's ScreenGui, holding `BarBg` with
a `BarFill` inside it, and a `Label`.

It is visible only while the player is seated in their vehicle, and the fill turns red below
twenty percent. Electric cars fill in the success colour and petrol cars in the accent
colour. See [The interface](interface.md).

## Turning it off

`Fuel.Enabled = false` disables the drain loop, the gauge, the station prompts and the
refuel path completely. Saved fuel levels stay on the profile and are used again if you turn
it back on.
