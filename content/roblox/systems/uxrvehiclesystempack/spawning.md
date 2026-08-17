---
title: Spawning and test drives
order: 5
description: Where a car appears, pad groups and occupancy, network ownership and timed loans.
icon: circle-parking
---

Spawning is server side. The client asks for a vehicle id, the server checks ownership, the
cooldown and placement, then clones the Model into `Workspace/uxrVehicleSystemWorkspace/Spawned`.

## One car at a time

Each player has exactly one active vehicle. Spawning a second despawns the first, and so
does losing your character.

| Event | Result |
|---|---|
| Spawning another car | The old one is destroyed |
| Dying or resetting | The car is destroyed |
| Leaving the server | The car is destroyed |
| Pressing the despawn key | The car is destroyed |

Any open trunk session on that car is closed at the same time.

## Free spawning

With no pad group, the car appears in front of the player, facing the way they face.

```lua
SpawnCooldown = 3,
SpawnClearance = 12,
```

The gap is half the car's largest horizontal dimension plus `SpawnClearance`, so a bus
lands further out than a hatchback. Height comes from the player's feet plus half the car's
height.

<Callout type="warning" title="Free spawning does not check for obstacles">

Nothing is raycast. Spawn while facing a wall and the car goes into the wall; spawn on a
bridge and it may appear under it.

Pads are the fix. Use free spawning for sandbox games and pads anywhere the world is
tight.

</Callout>

`SpawnCooldown` is per player and rejects with a "wait before spawning again" notice.

## Spawn pads

```lua
SpawnPads = {
    Enabled = true,
    GroupAttribute = "SpawnGroup",
    YawAttribute = "SpawnYaw",
    DefaultGroup = "",
    OccupancyPadding = 2,
    OccupancyHeight = 18,
    NearestFreePad = true,
},
```

A pad is any `BasePart` under the world folder carrying a string attribute named
`SpawnGroup`. Its value is the group name.

A vehicle row with `SpawnPads = "police"` may spawn **only** at pads in that group. A
vehicle with no `SpawnPads` field free spawns, unless `DefaultGroup` names a group, which
sends every untagged vehicle to pads too.

| Step | What happens |
|---|---|
| Collect | Every pad in the group |
| No pads at all | Refused with the "no spawn pad available" notice |
| Test occupancy | A box above each pad, `OccupancyPadding` wider and `OccupancyHeight` tall |
| All occupied | Refused with the "all pads occupied" notice |
| Choose | The nearest free pad, or the first if `NearestFreePad = false` |

Occupancy only counts things inside the `Spawned` folder, so a player standing on a pad
does not block it but somebody's parked car does. The player's own current car is ignored,
which is what lets you swap cars while standing on the only free pad.

### Facing

`SpawnYaw` is an optional number attribute on a pad, in degrees, rotating the car from the
pad's own orientation. Use it when the pad part is easier to build square than to build
aligned with the road.

<Callout type="tip" title="Pads are how you build a police garage">

Tag six parts in the station's garage with `SpawnGroup = "police"`, give every patrol car
`SpawnPads = "police"`, and the fleet can only ever appear inside the garage. Six pads means
six patrol cars out at once, and the seventh officer is told the bays are full.

The bay count becomes a fleet limit without any extra configuration.

</Callout>

## After the car lands

| Step | Setting |
|---|---|
| Paint and plate are applied | `Recolor`, `Plate` |
| The trunk prompt is attached | `Trunk` |
| Network ownership is wired | `ManageNetworkOwnership` |
| The player is seated | `AutoSit`, `AutoSitDelay` |

### Network ownership

```lua
ManageNetworkOwnership = true,
```

Every unanchored part is handed to whoever sits in the driver's seat, and returned to
automatic when they get out. That is what makes the car feel responsive instead of
rubber-banding.

Set it to `false` for chassis that manage ownership themselves. Two systems fighting over
ownership is worse than neither doing it.

### Auto sit

```lua
AutoSit = true,
AutoSitDelay = 0.6,
```

The player is dropped into the first `VehicleSeat` found in the model. The system first
waits for the seat to unanchor, up to five seconds, then waits `AutoSitDelay` on top.

<Callout type="info" title="What the delay is for">

Most chassis scripts connect their own seat listeners when the car spawns. Sitting a player
down before those listeners exist means the car does not know it has a driver.

If your cars sometimes spawn dead but drive fine after getting out and back in, raise
`AutoSitDelay`.

</Callout>

## Test drives

```lua
TestDriveEnabled = true,
TestDriveDuration = 30,
```

A timed loan of a car the player does not own. Same spawn path, same pad rules, plus a
timer that despawns the car when it expires.

| Override | Where |
|---|---|
| Block one car from being test driven | `TestDrivable = false` on the row |
| A different length for one car | `TestDriveDuration` on the row |
| Turn the feature off | `TestDriveEnabled = false` |

The countdown runs on the client HUD, and the despawn is scheduled on the server, so a
client that closes the HUD still loses the car on time.

A test drive earns nothing, which is checked every sample rather than at the start.

<Callout type="warning" title="A test drive needs stock access, not ownership">

The server checks that the storefront the request names actually sells the car and that the
player may open that storefront. It does not check the vehicle's own `Permissions`.

A locked supercar in a storefront the player can open is therefore test drivable even
though they cannot buy it. Usually that is the sales pitch. If it is not, set
`TestDrivable = false` on the locked rows, or move them to a gated storefront.

</Callout>

## Failure messages

| Reason | Means |
|---|---|
| Unknown | No catalog row with that id |
| NotOwned | They do not own it and it is not issued to them |
| Cooldown | Within `SpawnCooldown` of their last spawn |
| ModelMissing | No Model in `Storage/Vehicles` matching `ModelName` |
| NoPad | The vehicle's pad group has no pads in the world |
| PadsFull | Every pad in the group is occupied |

`ModelMissing` and `NoPad` are build mistakes rather than player mistakes. Both are worth
testing for every row before you publish.
