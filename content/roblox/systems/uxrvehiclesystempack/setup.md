---
title: Setup
order: 1
description: Install, build the world folder, add your first car, and sell it.
icon: play
---

Install with the standard four steps in
[Installing a system](../platform/installation.md): drag `uxrVehicleSystemPack` into
**ServerScriptService** and press Play once.

The bootstrap creates `Workspace/uxrVehicleSystemWorkspace`, which is both your build root
and the runtime spawn target.

## 1. The world folder

Every world part this system looks for must live under that one folder, at any depth. The
system searches nowhere else.

| Part or model | Named | Does |
|---|---|---|
| Dealership prompt | `DealershipZone`, `CivilianDealer`, `PoliceDealer` | Opens a storefront |
| Spawn pad | Any name, with a `SpawnGroup` attribute | Where restricted vehicles appear |
| Gas station | `GasStation` | Refuels petrol cars |
| Charger | `Charger` | Charges electric cars |
| Radar | A Model containing a part named `Sensor` | Measures and fines |
| Garage preview | `GarageModel` with a `SpawnPart` folder | Where the previewed car stands |

Organize them into subfolders however you like. The names are all configurable in
`Settings.luau`.

<Callout type="info" title="What survives a restart and what does not">

Instances you place in that folder by hand are preserved. Instances the system creates are
tagged with a `vspRuntime` attribute, and everything carrying that attribute is destroyed
and rebuilt on every server start.

The `Spawned` folder is runtime. Your roads, dealerships and pads are not.

</Callout>

## 2. Add a vehicle

Two steps, always in this order.

Drop a Model into `Storage/Vehicles`. It needs a `VehicleSeat` to be drivable, and the seat
is what the system watches for speed, earning and fuel.

Then add a row to `Shared/Config/VehicleData.luau`:

```lua
[7] = {
    Name = "Coastal Roadster",
    ModelName = "CoastalRoadster",
    Category = "Convertible",
    Brand = "Kestrel",
    Price = 12000,
    Description = "Two seats, no roof, no regrets.",
    ImageId = "",
    OnSale = true,
    Stats = { Speed = 78, Acceleration = 74, Handling = 81 },
},
```

`ModelName` must match the Model's name exactly. The table key is the persistent id that
ownership is saved against.

<Callout type="danger" title="Never reuse a vehicle id">

Ownership, saved colors, plates, fuel levels and trunk contents are all keyed on the table
key. Giving an old id to a new car hands that car to everybody who owned the old one, with
the old one's paint and luggage.

Deleting a row is safe. Recycling its key is not.

</Callout>

See [The catalog](catalog.md) for every field.

## 3. Decide where the money comes from

`Leaderstats.Enabled` ships as `true`, which points the whole system at your game's
`leaderstats.Cash` stat instead of keeping a balance of its own.

<Callout type="danger" title="Buying is broken while Leaderstats.Enabled is true">

The purchase check reads the internal profile balance directly rather than through the
balance accessor, and the internal balance is never written while leaderstats mode is on.
It stays at zero, so every priced vehicle is refused with the "you can't afford" notice no
matter how much money the player has. Free vehicles still work.

Until this is fixed, set `Leaderstats.Enabled = false` and let the system keep its own
balance. Refuelling, fines, auction bids and admin coin edits all read the balance
correctly; it is only the buy path that does not.

</Callout>

With leaderstats off, `StartingBalance` is granted on the first join and the system owns
the number. See [Money and earning](economy.md).

## 4. Set the Studio data mode

```lua
StudioMode = false, -- true | false | "auto"
```

The shipped value is `false`, which means **always persist**, including while you are
testing in Studio. Every test writes to the live profile store.

| Value | Behaviour |
|---|---|
| `true` | Never read or write. Every session starts fresh |
| `false` | Always read and write, in Studio and on live servers |
| `"auto"` | Fresh in Studio, persistent on live servers |

`"auto"` is what the comment above the setting recommends and what most people want. Change
it before your first playtest.

## 5. Your first sale

| Check | Where |
|---|---|
| Press G | The general dealership opens |
| The list has cars | `VehicleData` rows the player may see |
| Buying one works | With `Leaderstats.Enabled = false` and a balance |
| Spawning works | The Model exists in `Storage/Vehicles` |
| The car drives | Your own chassis scripts, not this system |

<Callout type="info" title="This system does not drive the car">

It spawns your Model, seats the player and hands them network ownership. Steering,
suspension, gearing and engine sound are whatever your chassis does.

If a car spawns and does not move, the problem is in the Model.

</Callout>

## What ships in the box

| | |
|---|---|
| Six catalog rows | Starter Hatch, City Sedan, Apex GT, Collector Coupe, Classic Sedan, Police Cruiser |
| Zero vehicle models | `Storage/Vehicles` is empty |
| Three dealerships | General, Civilian, Police |
| One radio track | A single `rbxassetid://` in `Controls.RadioPlaylist` |
| No trunk item templates | `Storage/TrunkItems` does not exist until you create it |
