---
title: The catalog
order: 2
description: One row per vehicle, the stat sheet, and the four ways to lock a car.
icon: list
---

`Shared/Config/VehicleData.luau` is the catalog. One table row per vehicle, keyed by a
number that is the vehicle's permanent identity.

```lua
[2] = {
    Name = "City Sedan",
    ModelName = "CitySedan",
    Category = "Sedan",
    Brand = "Kestrel",
    Dealerships = { "general", "civilian" },
    Price = 2500,
    Description = "Comfortable, dependable, unremarkable.",
    ImageId = "",
    OnSale = true,
    Stats = { Speed = 48, Acceleration = 45, Handling = 55 },
},
```

## Every field

| Field | Required | Does |
|---|---|---|
| `Name` | Yes | The display name everywhere in the UI |
| `ModelName` | Yes | The Model in `Storage/Vehicles` to spawn and preview |
| `Category` | Yes | The Type filter, and the label under the name |
| `Brand` | No | The Brand filter |
| `Dealerships` | No | Which storefronts stock it. Omitted means the general one only |
| `Price` | Yes | In your currency. `0` is free |
| `Description` | Yes | The blurb in the detail panel |
| `ImageId` | Yes | A thumbnail asset, or `""`. Cosmetic |
| `OnSale` | Yes | `false` shows it as offsale and refuses purchases |
| `Stats` | Yes | The stat sheet, keyed to `Settings.Stats` |
| `IsElectric` | No | Charges at a Charger instead of a gas station |
| `Colors` | No | The swatch palette for player recoloring |
| `RecolorMode` | No | Overrides `Settings.Recolor.Mode` for this car |
| `SpawnPads` | No | Restricts it to pads in this group |
| `TestDrivable` | No | `false` blocks test drives of this car |
| `TestDriveDuration` | No | Its own test drive length in seconds |
| `TrunkCapacity` | No | Its own trunk size, overriding the default |
| `Permissions` | No | The access lock |

## Brands and categories

Both are free text, and both drive filters in the dealership. `Category` doubles as the
detail label under the vehicle name.

<Callout type="warning" title="Do not use real car brands">

The shipped rows use invented names: Kestrel, Voltaic, Heritage, Sentinel. Real
manufacturer names on a Roblox storefront are a trademark problem, and the model that goes
with them usually is as well.

</Callout>

## The stat sheet

`Settings.Stats` decides which rows appear and in what order. Each row reads a key out of
the vehicle's `Stats` table.

```lua
Stats = {
    { Label = "Speed", Key = "Speed", Max = 100 },
    { Label = "Acceleration", Key = "Acceleration", Max = 100 },
    { Label = "Handling", Key = "Handling", Max = 100 },
},
```

| Field | Does |
|---|---|
| `Label` | The caption |
| `Key` | The key to read out of the vehicle's `Stats` |
| `Suffix` | Appended to the value |
| `Max` | Full scale for the bar. `0` shows the value with no bar |

Values may be numbers or strings. `Max = 0` is what makes a text stat work: a top speed of
`273` with a `" KM/H"` suffix, or a safety rating of `"5 STARS"`.

<Callout type="info" title="Stats are decoration">

Nothing in this system reads them. A car with `Speed = 100` is not faster than one with
`Speed = 30` unless your chassis makes it so.

They exist so the buyer can compare cars, so keep them honest against how the models
actually drive.

</Callout>

## Access locks

`Permissions` is how a car becomes staff-only, group-only or pass-only. Every field is
optional and they combine, so a car can require a team **and** a group rank.

```lua
Permissions = {
    GamepassId = 123, GamepassGrants = false,
    Teams = { "Police", "Staff" },
    GroupId = 456, GroupMinRank = 100,
    AutoGrant = true,
},
```

| Field | Means |
|---|---|
| `GamepassId` | Requires this gamepass |
| `GamepassGrants` | `true` flips it: pass owners get the car free, everybody else pays |
| `Teams` | Must be on one of these teams |
| `GroupId` and `GroupMinRank` | Must be in the group at that rank or above |
| `AutoGrant` | Anybody who passes the checks above owns it already, free |

Every configured dimension must pass. There is no OR.

### The badge

A locked card shows a badge naming the first reason it is locked: `GAMEPASS`, `TEAM` or
`GROUP`. Auto-granted cars show `Permissions.IssuedBadge`, which ships as `ISSUED`.

```lua
Permissions = { HideLocked = false, IssuedBadge = "ISSUED" },
```

`HideLocked = true` removes locked cars from the list entirely instead of showing them
badged. Showing them is usually better: a locked supercar is an advert for the gamepass.

## Duty vehicles

The pattern worth copying is the shipped Police Cruiser.

```lua
[6] = {
    Name = "Police Cruiser",
    ModelName = "PoliceCruiser",
    Category = "Patrol",
    Dealerships = { "police" },
    SpawnPads = "police",
    Permissions = { Teams = { "Police" }, AutoGrant = true },
    Price = 0,
    OnSale = true,
    Stats = { Speed = 80, Acceleration = 75, Handling = 78 },
},
```

Four restrictions working together: only the police dealership stocks it, only the police
team may access it, officers own it without buying it, and it only ever appears on police
pads.

<Callout type="info" title="Issued is not owned">

An auto-granted car is not written to the player's profile. Eligibility is re-checked every
time, so leaving the team takes the car away and the profile stays clean.

That also means it never counts against a garage slot.

</Callout>

## Free versus offsale

| You want | Set |
|---|---|
| A free starter car | `Price = 0`, `OnSale = true` |
| A car nobody can buy any more | `OnSale = false` |
| A car only pass owners may buy | `Permissions.GamepassId`, `GamepassGrants = false` |
| A car pass owners get free | `Permissions.GamepassId`, `GamepassGrants = true` |
| A car issued to a team | `Permissions.Teams` and `AutoGrant = true` |

`StarterVehicleId` in `Settings.luau` grants one car on the player's first join, and ships
pointing at row `1`. Set it to `nil` for nobody to start with anything.

<Callout type="warning" title="An offsale car can still be auctioned">

`OnSale = false` blocks the dealership, not the auction house. A player who already owns a
retired car can list it, and the winner ends up owning something no longer for sale.

That is usually the point of a collector car. If it is not, disable auctions or keep
retired cars out of the catalog entirely.

</Callout>
