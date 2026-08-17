---
title: uxrVehicleSystemPack
order: 30
description: A vehicle dealership economy with a catalog, ownership, spawn pads, fuel, radar, trunks and auctions.
icon: car
---

A full vehicle economy in one folder. Players browse a dealership, buy cars with your
game's currency, spawn them on pads, recolor them, plate them, fuel them, fill their
trunks, get fined by roadside radars and auction them off to each other.

Two config files hold everything a designer touches: `Settings.luau` for the system and
`VehicleData.luau` for the catalog. Everything else is engine code.

## Pages

- [Setup](setup.md)
- [The catalog](catalog.md)
- [Dealerships](dealerships.md)
- [Money and earning](economy.md)
- [Spawning and test drives](spawning.md)
- [Color and plates](customization.md)
- [Fuel and charging](fuel.md)
- [Speed radar](radar.md)
- [Trunks](trunk.md)
- [The auction house](auction.md)
- [In car controls and the HUD](controls.md)
- [The admin panel](admin.md)
- [The interface](interface.md)
- [Settings](settings.md)

## What it does

| | |
|---|---|
| Catalog | One row per vehicle: price, stats, brand, category, access rules |
| Dealerships | As many storefronts as you like, each with its own access gate and stock |
| Ownership | Server-authoritative, saved per player, keyed on the vehicle id |
| Money | Its own balance, or your game's `leaderstats` stat |
| Earning | Coins per second while driving, with Premium, gamepass, group and team multipliers |
| Spawning | Free spawn in front of the player, or pad groups with occupancy checks |
| Test drives | A timed loan of a car the player does not own |
| Customization | Body recolor from a palette or a free picker, plus typed number plates |
| Fuel | Litres or kWh, draining while driving, refuelled at a station part |
| Radar | Roadside speed boards that fine drivers and flash their lights |
| Trunks | Per-vehicle tool storage with sharing and optional police inspection |
| Auctions | List an owned car, other players bid, the top bid wins |
| Admin | An in-game panel for coins, vehicles and profile wipes |

## The shape of it

The package installs itself. On the first server start it copies `Shared` into
`ReplicatedStorage`, moves `Storage` into `ServerStorage`, deploys the ScreenGui into
`StarterGui`, creates the remotes and builds a single Workspace folder that holds both your
build and the runtime spawn folder.

Everything the player touches goes through one RemoteFunction. There is no
`OnServerEvent` handler at all: the RemoteEvent is server to client only, so a client
cannot invent an event the server will act on.

<Callout type="warning" title="You supply the vehicles">

The package ships six catalog rows and no models. `Storage/Vehicles` is an empty folder
waiting for your Models, named to match each row's `ModelName`.

A row whose model is missing fails at spawn time with a "no model" notice, so the catalog
and the folder have to be kept in step.

</Callout>
