---
title: Settings
order: 10
description: The fifteen config files, persistence, global counters, and the folder layout.
icon: sliders-horizontal
---

## The config files

| File | Holds |
|---|---|
| `Settings.luau` | The switches on this page |
| `Pets.luau` | Every pet |
| `Rarities.luau` | Rarity colours and tiles |
| `Variants.luau` | Golden, Rainbow, Huge and their multipliers |
| `Gradients.luau` | Named colour blends |
| `Eggs.luau` | Eggs and their pools |
| `EggAnim.luau` | The open animation timings |
| `Merge.luau` | Merge recipes |
| `IndexGoals.luau` | Collection milestones |
| `Follow.luau` | How pets trail behind the player |
| `Breakables.luau` | Breakable types |
| `Zones.luau` | Which breakables fill which zone |
| `Machines.luau` | World objects that open panels |
| `Monetization.luau` | Gamepass ids and luck tiers |
| `Sounds.luau` | Interface sounds |

## Settings

| Field | Default | What it does |
|---|---|---|
| `Debug` | `true` | Extra `[uxrPS]` lines in Output |
| `StudioMode` | `"auto"` | Whether pets save |
| `Currency` | `{ Key = "Coins", Start = 100000 }` | The main currency |
| `Currencies` | `{ Gems = { Start = 500 } }` | Extra currencies |
| `CurrencyUI` | Enabled, showing Coins and Gems | The corner boxes |
| `PriceIcons` | Three ids | Icons per currency, plus Robux |
| `MaxEquipped` | `8` | Base equip cap |
| `BackpackSize` | `500` | Base inventory cap |
| `Passes` | Two entries | Gamepasses granting extra slots |
| `PetStack` | `true` | Group identical pets in the inventory |
| `AutoEquip` | Both `true` | Equip new pets automatically |
| `BoostMode` | `"sum"` | How equipped power combines |
| `HatchCooldown` | `0.5` | Minimum seconds between opens |
| `DefaultLuck` | `1` | Luck with no gamepass |
| `Global` | Enabled, 15-second flush | Server-wide counters |
| `Announce` | Enabled, global | Rare hatch messages |
| `Trade` | `{ ConfirmSeconds = 5 }` | The trade countdown |

<Callout type="warning" title="Debug ships as true">

Turn it off before release. Hatching, damage and syncing all log while it is on, and a busy
server produces a great deal of output.

</Callout>

## Persistence

Profiles go to the DataStore `uxrPS_Profile_v1` and hold the pets, the equipped list, the
index and the purchase records.

```lua
StudioMode = "auto",
```

| Value | Behaviour |
|---|---|
| `"auto"` | Saves in a published game, fresh slate every Studio run |
| `false` | Always saves, including Studio. Needs API services enabled |
| `true` | Never saves |

<Callout type="info" title="The comments in the file describe true and false the other way around">

The shipped comment reads `true = never saves` and `false = always saves`, which sounds
inverted but matches the code exactly: `true` forces the fresh-slate mode, and `false`
forces real saving.

`"auto"` is what you want while building, and what ships.

</Callout>

A profile whose load failed is marked as not loaded and is never saved over, so a DataStore
outage cannot wipe somebody's collection.

Every pet has an internal id from a per-profile counter, which is why a traded pet arrives
with a new one.

## Global counters

```lua
Global = { Enabled = true, FlushInterval = 15 },
```

Two server-wide numbers, shared across every server through a DataStore and
`MessagingService`:

| Counter | Tracks |
|---|---|
| Total hatched | How many pets have ever been opened |
| Per-pet existence | How many of each species and variant exist right now |

The existence count is what lets the interface show how rare a pet actually is, rather than
only its odds. It is incremented on hatch and on merge output, and decremented on delete and
on merge input.

`FlushInterval` is how often pending changes are written and broadcast. Lower means fresher
numbers and more DataStore traffic; 15 seconds is a sensible middle.

`Enabled = false` turns both off, and the interface simply shows nothing for them.

## The folder layout

The bootstrap builds this on every server start:

| Location | Contains |
|---|---|
| `ServerScriptService/uxrPetSystem` | The package. The only design-time copy |
| `ReplicatedStorage/uxrPetSystem` | `Shared/`, plus copies of the pet and egg models |
| `ServerStorage/uxrPetSystem` | `Storage/`, the authoritative models |
| `Workspace/uxrPetSystemWorkspace` | `Spawned` for pets, `Breakables`, and your `Zones` |
| `StarterGui/uxrPetSystemGui` | The interface |

<Callout type="danger" title="Only the ServerScriptService copy survives a restart">

Every other location is wiped and recreated when the server starts. A model dropped into
`ServerStorage/uxrPetSystem` at runtime, or a UI change made in `StarterGui`, is gone on the
next run.

Edit the package's own copies inside `ServerScriptService`. That is the source of truth.

The exception is your `Zones` folder in the workspace, which you build yourself and which is
left alone.

</Callout>

## The interface

`uxrPetSystemGui` in `StarterGui`, with panels for the inventory, the index, the egg board,
merging, trading and the currency display.

Restyle it in `ServerScriptService/uxrPetSystem/Client/uxrPetSystemGui`. The bootstrap
copies it into `StarterGui` at run time, and only if a same-named ScreenGui is not already
there.

## Where to start

| You want | Edit |
|---|---|
| A new pet | `Config/Pets.luau` and a model in `Storage/Pets` |
| A new egg | `Config/Eggs.luau` and a tagged platform |
| Different odds | The pool weights in `Config/Eggs.luau` |
| A harder coin zone | `Config/Breakables.luau` and `Config/Zones.luau` |
| Bigger squads | `MaxEquipped`, or the `Passes` list |
| Different pet movement | `Config/Follow.luau` |
| Your own economy | The economy adapter. See [Currencies and gamepasses](monetization.md) |
| To hook it into your game | [The server API](api.md) |
