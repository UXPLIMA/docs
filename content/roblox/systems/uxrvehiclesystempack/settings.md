---
title: Settings
order: 14
description: The two config files, the message templates, the profile store and the folder layout.
icon: sliders-horizontal
---

## The two files

| File | Holds |
|---|---|
| `Shared/Config/Settings.luau` | The system |
| `Shared/Config/VehicleData.luau` | The catalog |

Nothing else in the package is meant to be edited. Both files are replicated to
`ReplicatedStorage`, so every value in them is readable by any client.

<Callout type="danger" title="Nothing in Config is secret">

`Settings.luau` is shipped to every player, including `Admin.Ids`, the group ids, the
gamepass ids and every price.

None of those are secrets in Roblox terms, so this is fine as it stands. It is worth
knowing before you add an API key or a webhook URL to the file: there is no server-only
config folder in this system, and anything you put there is public.

</Callout>

## The map

| Block | Covers | Page |
|---|---|---|
| `Debug`, `StudioMode` | Logging and Studio data | Below |
| `Currency`, `StartingBalance`, `StarterVehicleId`, `Leaderstats` | The balance | [Money and earning](economy.md) |
| `Earn` | Coins for driving | [Money and earning](economy.md) |
| `DriveHud` | The driving readouts | [In car controls and the HUD](controls.md) |
| `Radar` | Speed boards and fines | [Speed radar](radar.md) |
| `SpawnCooldown`, `SpawnClearance`, `AutoSit`, `ManageNetworkOwnership` | Spawning | [Spawning and test drives](spawning.md) |
| `TestDriveEnabled`, `TestDriveDuration` | Timed loans | [Spawning and test drives](spawning.md) |
| `SpawnPads` | Pad groups | [Spawning and test drives](spawning.md) |
| `WorldFolder` | Where the build lives | Below |
| `ProximityZoneName`, `OpenKey`, `HideToolsInDealership`, `FocusMode` | Opening the panel | [In car controls and the HUD](controls.md) |
| `NotifyStyle` | Toast text | Below |
| `Dealerships` | Storefronts | [Dealerships](dealerships.md) |
| `VehicleFolder`, `ShowroomFolder` | Asset folders | Below |
| `Stats` | The stat sheet | [The catalog](catalog.md) |
| `Recolor`, `Plate` | Customization | [Color and plates](customization.md) |
| `Fuel` | Fuel and charging | [Fuel and charging](fuel.md) |
| `Permissions` | Locked cars | [The catalog](catalog.md) |
| `Trunk` | Storage | [Trunks](trunk.md) |
| `Vip`, `DailyDeal`, `FlashSale`, `TradeIn` | Discounts | [Dealerships](dealerships.md) |
| `Garage` | Slot limits | [Money and earning](economy.md) |
| `Preview` | The 3D turntable | Below |
| `Auction` | The auction house | [The auction house](auction.md) |
| `Controls` | The in-car widget | [In car controls and the HUD](controls.md) |
| `Admin` | The admin panel | [The admin panel](admin.md) |
| `Messages` | Every line of text | Below |

## Debug and Studio data

```lua
Debug = true,
StudioMode = false,
```

`Debug` ships on and prints routine activity to Output. Turn it off before you publish;
warnings and errors are printed either way.

`StudioMode` decides whether the profile DataStore is used.

| Value | Behaviour |
|---|---|
| `true` | Never read or written. Every session starts fresh |
| `false` | Always read and written, including in Studio |
| `"auto"` | Fresh in Studio, persistent on live servers |

The shipped value is `false`, which means your Studio tests write to the same store as your
live game. Change it to `"auto"` before you start building.

## The profile

Everything a player owns is one DataStore entry, keyed on their user id.

| | |
|---|---|
| Store | `uxrVDS_Profile_v1` |
| Holds | Balance, owned ids, paint, plates, fuel levels, trunk contents, trunk access |
| Saved | On leave, on shutdown, and every 60 seconds |

<Callout type="info" title="A failed load never overwrites good data">

If the read fails, the profile is marked as not loaded and every save is skipped for that
session. The player sees an empty garage for that session, which is annoying, and their real
garage is untouched, which is the important part.

The failure is logged as an error, so a run of them in Output means Roblox datastores are
having a bad day rather than your configuration being wrong.

</Callout>

There is no session lock. Two servers holding the same player at once would each save their
own copy, and the last write wins. In practice Roblox does not put one player in two
servers, so this only matters if you are moving players between places quickly.

## The world folder

```lua
WorldFolder = "uxrVehicleSystemWorkspace",
VehicleFolder = "Vehicles",
ShowroomFolder = "Showroom",
```

`WorldFolder` is the one place in `Workspace` this system looks. Dealership prompt parts,
spawn pads, gas stations, chargers, radars and the garage model all live inside it, at any
depth.

`VehicleFolder` and `ShowroomFolder` name folders inside `Storage`. Vehicles are the models
the catalog spawns; the showroom is scenery that is moved into the world folder at startup
and tagged as runtime, so it is rebuilt every restart.

## The garage preview

```lua
Preview = {
    Enabled = true,
    ModelName = "GarageModel",
    SpawnFolderName = "SpawnPart",
    PadIndex = 1,
    RadiusScale = 1.6,
    MinPitch = -20,
    MaxPitch = 60,
    OrbitSensitivity = 0.01,
    Zoom = { Enabled = true, Min = 8, Max = 35, Step = 3 },
    CameraTween = 0.4,
    FreezeCharacter = true,
    CameraHeight = { Enabled = true, Min = -235.09, Max = -190.76 },
},
```

The selected car is spawned client side on a pad inside `GarageModel` and the camera orbits
it while the dealership is open. Nobody else sees it.

<Callout type="warning" title="CameraHeight is tied to where the garage is">

`Min` and `Max` are world-space Y values, and the shipped numbers describe the position of
the demo garage. Move your garage and the camera will clamp itself to a height that is
nowhere near the car.

Either update both numbers to your garage's own height, or set
`CameraHeight.Enabled = false` and let the pitch limits do the work.

</Callout>

`FreezeCharacter` pins the player in place while previewing, which stops them walking away
from their own camera.

## Messages

Every line of text the system generates is a template in `Settings.Messages`. Around a
hundred of them: toast titles, purchase results, filter captions, button captions, fuel
labels, auction rows and radar banners.

Tokens are written with a leading `$` and replaced at display time.

| Token | Becomes |
|---|---|
| `$vehicle` | The vehicle's name |
| `$amount` | A formatted price |
| `$req` | What a locked car requires |
| `$count` | A count, such as garage slots used |
| `$plate` | The plate text |
| `$fuel` | The word fuel or the word charge |
| `$name`, `$item`, `$time`, `$speed`, `$limit`, `$used`, `$max` | Their obvious values |

A token with no value is left in place rather than blanked, so a typo shows up on screen as
`$vehicle` instead of vanishing.

Translating the system is editing this one block. Static button captions that never change
live on the ScreenGui in Studio instead.

## Notification style

```lua
NotifyStyle = {
    Font = Enum.Font.GothamMedium,
    TextSize = 18,
    Stroke = false,
},
```

Notifications are rendered by the companion notify system. Colours come from each message's
kind rather than from here; these three control the text.

## The folder layout

| Location | Contains |
|---|---|
| `ServerScriptService/uxrVehicleSystemPack` | The package. The only design-time copy |
| `ReplicatedStorage/uxrVehicleSystemPack` | `Shared`, the remotes, and a copy of the vehicle models |
| `ServerStorage/uxrVehicleSystemPack` | `Storage`, with vehicles, showroom and trunk items |
| `Workspace/uxrVehicleSystemWorkspace` | Your build, plus the runtime `Spawned` folder |
| `StarterGui/uxrVehicleSystemPackGui` | The interface |

Everything except the first is recreated at runtime. Edits made to the replicas at runtime
are lost on the next server start.

<Callout type="warning" title="Vehicle models are replicated to every client">

The bootstrap copies `Storage/Vehicles` into `ReplicatedStorage` as well, because the garage
preview spawns the selected car on the client.

That means every model in the catalog is downloaded by every player at join time. A large
catalog of heavy models is a join-time cost, so keep the models tidy and strip anything the
preview does not need.

</Callout>

## The remotes

One RemoteEvent and one RemoteFunction, under `Core/vspEvents`, multiplexed by a leading
string.

| Remote | Direction |
|---|---|
| `RemoteFunction` | Client asks, server answers. Every action goes through it |
| `RemoteEvent` | Server tells the client. Balance, ownership, fuel, trunk, auctions, fines |

The RemoteEvent has no server-side listener at all, so there is no path for a client to
fire an event the server acts on. Every request is a function call the server validates.

## Where to start

| You want | Edit |
|---|---|
| Different cars | `VehicleData.luau` |
| A working purchase path | `Leaderstats.Enabled = false` |
| Safe Studio testing | `StudioMode = "auto"` |
| A slower fuel burn | `Fuel.DrainPerSecond` |
| Different speed limits | `Radar.MaxSpeed`, or a `Configuration` per radar |
| More money for driving | `Earn.RewardPerSecond` and `Earn.Multipliers` |
| A police fleet | `Permissions.Teams` with `AutoGrant`, plus `SpawnPads` |
| Any tool in the trunk | `Trunk.AllowedItems = {}` |
| Another language | `Settings.Messages` |
