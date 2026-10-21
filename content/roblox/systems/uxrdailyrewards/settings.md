---
title: Settings
order: 2
description: Every field in Settings.luau with its default and what changing it does.
icon: sliders-horizontal
---

`Shared/Config/Settings.luau`. Change the values on the right of the `=`, never the keys
on the left.

This file is copied to `ReplicatedStorage`, so the client can read it. Nothing private
belongs here.

## Top level

| Field | Default | What it does |
|---|---|---|
| `Debug` | `false` | Extra `[uxrDR]` lines in Output |
| `Currency` | `"$"` | Symbol the interface puts next to amounts |
| `StartingBalance` | `0` | Only read if your own reward code reads it |
| `AssetFolder` | `"Assets"` | Folder under `ServerStorage/<package>` for designer models |

`StartingBalance` and `AssetFolder` exist because the skeleton is shared across the
product line. Neither does anything on its own in this system.

## DailyRewards

| Field | Default | What it does |
|---|---|---|
| `ClaimIntervalSeconds` | `86400` | Seconds a player waits between claims |
| `ResetStreakOnMiss` | `true` | Send a player back to day 1 after a missed window |
| `ResetAfterMissedDays` | `1` | Whole extra intervals that must pass before that reset |
| `LoopAfterLastDay` | `true` | Restart at day 1 after the last day is claimed |
| `AutoOpenOnJoin` | `true` | Open the panel on join, but only when a reward is ready |

`ResetStreakOnMiss = false` makes the ladder forgiving: a player who disappears for a
week comes back to the day they were on. See [Streaks and cooldowns](streaks.md) for
exactly when the reset fires.

`AutoOpenOnJoin` never opens an empty panel. A player still on cooldown joins to a closed
interface.

## Skip

| Field | Default | What it does |
|---|---|---|
| `Enabled` | `true` | Show the skip button at all |
| `DevProductId` | `0` | Your Developer Product id. `0` disables the feature |
| `PriceText` | `"99 R$"` | Button label. Cosmetic, match it to your product |
| `ManageProcessReceipt` | `true` | Let this system own `MarketplaceService.ProcessReceipt` |

The skip button only appears when `Enabled` is `true` **and** `DevProductId` is greater
than zero. Setting one without the other shows nothing.

`ManageProcessReceipt` matters the moment a second thing in your game sells a Developer
Product. [Skip the wait](skip.md) covers both cases.

## Profile

| Field | Default | What it does |
|---|---|---|
| `Store` | `"uxrDR_Profile_v1"` | `DataStore` name. Changing it wipes everyone |
| `AutosaveSeconds` | `60` | How often an in-memory profile is written back |

## Sounds

| Field | Default | What it does |
|---|---|---|
| `Enabled` | `true` | Master switch |
| `Volume` | `0.5` | Playback volume |
| `Claim` | `""` | Sound id played on a successful claim |
| `Open` | `""` | Sound id played when the panel opens |

Empty strings ship as the default, so the system is silent until you paste ids in.

## Messages

Four strings the interface shows. Edit the text freely, including translating it.

| Key | When it shows |
|---|---|
| `Claimed` | A claim succeeded |
| `AlreadyClaimed` | A claim was refused because the cooldown has not elapsed |
| `StreakReset` | The claim that just happened started a fresh cycle at day 1 |
| `SkipPurchased` | The skip product was bought and the timer cleared |

<Callout type="warning" title="Changing Profile.Store is not reversible from the game">

Every player's streak and last claim time is keyed by this name. Renaming it does not
migrate anything: it points the system at an empty store and everyone begins again at day
1. That is exactly what you want after a test session and exactly what you do not want on
a live game.

</Callout>
