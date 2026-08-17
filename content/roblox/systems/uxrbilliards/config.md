---
title: Configuration
order: 7
description: The two configuration files, what belongs in each, and every field at a glance.
icon: sliders-horizontal
---

This system has **two** configuration files, which is worth knowing before you go looking
for a field.

| File | Holds |
|---|---|
| `Shared/Billiards/Config.luau` | Everything about the game: physics, rules timing, AI, rewards, rating, leaderboards, Robux |
| `Shared/Config/Settings.luau` | The skeleton settings: debug, currency symbol, starting balance, asset folder, shop message wording |

Almost everything you want is in the first one.

## Shared/Config/Settings.luau

| Field | Default | What it does |
|---|---|---|
| `Debug` | `false` | Extra `[uxrBL]` lines in Output. Errors always show |
| `Currency` | `"$"` | Symbol beside prices |
| `StartingBalance` | `0` | Coins a new player begins with |
| `AssetFolder` | `"Assets"` | The folder under `Storage`. Leave as is |
| `Messages.PurchaseSuccess` | `"Purchased $item."` | `$item` is replaced with the item name |
| `Messages.Insufficient` | `"You can't afford $item."` | |
| `Messages.Unknown` | `"That item doesn't exist."` | |

## Shared/Billiards/Config.luau

### Table

| Field | Default |
|---|---|
| `tableConfig.pocketRadius` | `0.45` |
| `tableConfig.ballRadius` | `0.22` |

### Physics

| Field | Default |
|---|---|
| `params.friction` | `1.8` |
| `params.restitution` | `0.70` |
| `params.spinFactor` | `3.0` |
| `params.dt` | `1/240` |
| `params.stopThreshold` | `0.03` |
| `params.maxTime` | `12` |
| `params.sampleEvery` | `4` |

See [Physics and aiming](physics.md).

### Shot

| Field | Default |
|---|---|
| `shotClockSeconds` | `30` |
| `soloShotClock` | `false` |
| `maxPower` | `28` |
| `minPower` | `2` |

### Sound

| Field | Default |
|---|---|
| `sounds.single` | Four ids, picked at random |
| `sounds.hole` | Three ids, picked at random |
| `sounds.volume` | `0.45` |
| `sounds.minImpact` | `0` |
| `sounds.fullImpact` | `13` |
| `sounds.minVolScale` | `0.6` |
| `sounds.clusterWindow` | `0.06` |

### AI

| Field | Default |
|---|---|
| `ai.difficulty` | `"medium"` |
| `ai.aimError` | `{ easy = 0.09, medium = 0.045, hard = 0.015 }` |
| `ai.thinkSeconds` | `1.0` |
| `ai.UseFriendAvatar` | `true` |
| `ai.FallbackUsername` | a specific account |

See [The AI opponent](ai.md).

### Economy and rating

| Field | Default |
|---|---|
| `rewards.winCoins` | `50` |
| `rewards.playCoins` | `5` |
| `elo.start` | `1000` |
| `elo.K` | `32` |
| `leaderstats.enabled` | `true` |
| `leaderstats.elo` | `"Elo"` |
| `leaderstats.wins` | `"Wins"` |
| `coinProducts` | `{}` |
| `vipGamepass` | `0` |

### Leaderboards

| Field | Default |
|---|---|
| `leaderboard.stats` | `{ "Wins", "Losses", "Draws", "Elo" }` |
| `leaderboard.topN` | `25` |
| `leaderboard.refreshSeconds` | `60` |
| `leaderboard.storePrefix` | `"uxrBL_LB_"` |
| `leaderboard.storeVersion` | `"v1"` |
| `leaderboard.workspaceFolder` | `"uxrBilliardsWorkspace"` |
| `leaderboard.boardsFolder` | `"Leaderboards"` |

See [Rating, rewards and cosmetics](economy.md).

## The cosmetics catalogue

Cues are not in either settings file. They live in
`Shared/Billiards/Cosmetics.luau` as a list, because each entry carries code-shaped data
(how the cue is applied) rather than plain values.

## Where the specs are

`Shared/Billiards/_spec/` and `Server/Billiards/_spec/` hold the shipped test cases for
the physics, the rules, the engine, the aim maths, the rating and the leaderboard
rendering.

They need the Roblox API, so they run inside Studio with the test project connected:

```lua
require(game.ReplicatedStorage.uxrBilliardsSystem.Billiards._spec.Run)()
```

The everyday Rojo project excludes them, so they never reach a published place.
