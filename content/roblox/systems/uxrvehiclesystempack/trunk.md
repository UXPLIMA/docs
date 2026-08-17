---
title: Trunks
order: 9
description: Tool storage per vehicle, what persists, sharing access and police inspection.
icon: package
---

A trunk is per vehicle tool storage. The owner opens it from a prompt on the car, moves
tools in and out of their backpack, and can hand access to other players.

```lua
Trunk = {
    Enabled = true,
    ItemsFolder = "TrunkItems",
    PromptPartName = "Trunk",
    PromptText = "Open Trunk",
    DefaultCapacity = 20,
    PoliceInspect = { Enabled = false, Teams = { "Police" } },
    AllowSharing = true,
    AllowedItems = { "TestKit" },
},
```

## Giving a car a trunk

Put a `BasePart` named `Trunk` anywhere in the vehicle model. That is the whole
requirement, and a model without one simply has no trunk.

The part is usually invisible and sits behind the boot lid. The prompt is created on it at
spawn time, on the server, with a ten stud activation distance.

Capacity is `TrunkCapacity` on the catalog row, or `DefaultCapacity` for anything that does
not set one. It counts items, not weight or volume.

## What may go in

```lua
AllowedItems = { "TestKit" },
```

<Callout type="danger" title="The shipped list allows exactly one item">

A non-empty `AllowedItems` is a whitelist, and it ships containing a single demo tool name.
Out of the box that means nothing else in your game can be stored: every other tool is
refused with a "cannot be stored" notice.

Set `AllowedItems = {}` to allow any tool, or replace the list with the tools you actually
want carried in a car.

</Callout>

## What survives a rejoin

Two different things, and the difference matters.

| | |
|---|---|
| A tool in the trunk | Kept alive on the server for the session |
| A tool that **persists** | Must have a template in `Storage/TrunkItems` |

Only a count of names is saved to the profile, and only for names that have a matching
`Tool` template in that folder. On the next join the trunk is rebuilt by cloning templates.

<Callout type="warning" title="The TrunkItems folder does not ship">

`Storage/TrunkItems` does not exist in the package. Until you create it and drop `Tool`
templates in, nothing stored in any trunk survives a rejoin.

The trunk UI marks each row as saveable or not, so a player can see which of their items
are only along for the ride.

</Callout>

This design is deliberate. Rebuilding from templates means a tool's script updates the next
time the player loads it, and a tool you remove from the game stops coming back rather than
lingering in a thousand trunks.

## Who may open it

| Who | When |
|---|---|
| The owner | Always |
| A shared player | The owner granted them access |
| A police team | Only if `PoliceInspect.Enabled` |

```lua
PoliceInspect = { Enabled = false, Teams = { "Police" } },
```

Police inspection opens **any** vehicle's trunk, including a locked one, and takes items
out. That is a search, and it is off by default because it is a large power to hand out.

Turn it on for a roleplay game where contraband in a boot is meant to be findable, and pair
it with an `AllowedItems` list that actually contains the contraband.

### Sharing

```lua
AllowSharing = true,
```

The owner sees a share panel listing everybody in the server. Access is saved on the
owner's profile per vehicle, so it survives a rejoin and outlives the session that granted
it.

Shared players may take items out as well as put them in. There is no read-only level.

<Callout type="info" title="Access is by user id, not by session">

Somebody granted access keeps it until the owner removes it, across sessions and across
servers.

Worth mentioning in your own game's rules: it is a spare key, not a one-time favour.

</Callout>

## The session

Opening the trunk starts a session. Everybody looking at the same trunk sees changes live,
so two players moving items around do not fight over a stale list.

A session ends when the player closes the panel, when the car despawns, or when they leave.
Despawning the car closes it for everybody who had it open.

## When the owner leaves

Every tool held in that player's trunks is destroyed and the in-memory trunks are dropped.
The saved counts remain on the profile, so anything with a template comes back on the next
join and anything without one is gone.

That also means a shared player cannot keep using a trunk after its owner has left the
server, which is the behaviour you want.

## Turning it off

`Trunk.Enabled = false` creates no prompts, opens no sessions and saves nothing. Existing
saved contents stay on profiles.
