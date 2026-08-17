---
title: Setup
order: 1
description: Install, replace the gamepass ids, drop in models, and hatch your first pet.
icon: play
---

Install with the standard four steps in
[Installing a system](../platform/installation.md): drag `uxrPetSystem` into
**ServerScriptService** and press Play once.

The bootstrap creates a `uxrPetSystemWorkspace` folder in `Workspace` with a `Spawned`
folder inside it, and copies your models where the client and server can reach them.

## 1. Replace the gamepass ids

`Config/Monetization.luau` and `Config/Settings.luau` ship with real gamepass ids that
belong to somebody else.

| File | Ids |
|---|---|
| `Config/Monetization.luau` | `Open3x`, `Open8x`, `AutoOpen`, `FastOpen`, `Luck1`, `Luck2`, `Luck3` |
| `Config/Settings.luau` | The two entries in `Passes` |
| `Config/Eggs.luau` | `Pet2.devProductId` |

<Callout type="danger" title="These ids are not yours">

A player who owns the original creator's pass gets its benefit in your game, and a player
who buys one from your prompt has paid somebody else.

Replace every id with your own, or set it to `0` to disable that feature. Do this before you
publish, not after.

</Callout>

## 2. Drop in your models

| Folder | Contains |
|---|---|
| `Storage/Pets` | One `Model` per pet, named to match the key in `Config/Pets.luau` |
| `Storage/Eggs` | One `Model` per egg, named to match each egg's `modelName` |
| `Storage/Breakables/<Type>` | The damage stages, named `<Type>1` upward |

Pet models are cloned into `ReplicatedStorage` as well, so the client can show them in the
inventory previews.

## 3. Press Play

You start with 100,000 Coins and 500 Gems, which is deliberate: it is enough to test every
egg without editing anything.

The currency boxes appear in the corner, and the pet panel opens from the UI.

## 4. Make an egg platform

A `Model` in the world with:

| Part | |
|---|---|
| The tag `EggPlatform` | On the model |
| A `EggId` string attribute | Matching a key in `Config/Eggs.luau`, such as `Pet1` |
| A part named `ProximityPart` | Where the prompt appears. Falls back to the model's first part |

A `ProximityPrompt` named `OpenPrompt` is created for you, reading `Hatch` and the egg's
name.

Optional extras get the price stamped onto them automatically:

| Instance | |
|---|---|
| `InfoPart.InfoText` | An icon and a price label |
| `Plate.Plane.PriceGui` | The same, on the platform surface |

## 5. Hatch

Walk up and press the prompt. The egg shakes, pops, and a pet appears with its rarity
colour behind it.

If a free equip slot exists it equips itself and starts following you, because `AutoEquip`
ships on.

## 6. Make a coin zone

| Instance | |
|---|---|
| `Workspace/uxrPetSystemWorkspace/Zones/Zone1/Zone` | A flat `BasePart` covering the area |
| `Storage/Breakables/Coins/Coins1`, `Coins2`, `Coins3` | The damage stages |

Eight coin piles spread themselves across the zone. Click one and your pets run over and
start smashing it.

The highest-numbered stage is the healthy look, and the model swaps down as the health
drops.

## 7. Adjust the economy

The shipped numbers are a demo, not a balance:

| Setting | Ships as | Reality |
|---|---|---|
| `Currency.Start` | `100000` | Far too generous for a live game |
| `Gems.Start` | `500` | Same |
| `Pet4` price | `1` Coin | A guaranteed Huge for one coin |
| `Coins` reward | `1200` per pile, 4-second respawn | Very fast |

Set these to whatever your progression actually needs before launch.

## 8. Turn Debug off

`Settings.Debug` ships as `true`. Set it to `false` before release.

## 9. Publish

Pets save to a DataStore. `StudioMode = "auto"` gives every Studio session a clean slate,
so persistence is something you check in the published game.
