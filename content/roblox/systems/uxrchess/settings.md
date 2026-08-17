---
title: Settings
order: 3
description: Time controls, feature switches, highlights, sounds and messages, with defaults.
icon: sliders-horizontal
---

`Shared/Config/Settings.luau`.

## Identity

| Field | Default | What it does |
|---|---|---|
| `Debug` | `true` | Extra `[uxrCH]` lines in Output |
| `Currency` | `"$"` | Symbol beside amounts |
| `StartingBalance` | `0` | Balance a new profile begins with |
| `AssetFolder` | `"Assets"` | Designer models folder under `ServerStorage/<package>` |
| `Tag` | `"ChessTable"` | The tag your table models carry |

`Messages` holds three shop strings that the chess game itself never uses. They are there
for a shop built on top of the same profile.

## Time controls

```lua
TimeControls = {
    { key = "untimed",   label = "Untimed",    ms = nil },
    { key = "blitz5",    label = "Blitz 5m",   ms = 5 * 60 * 1000 },
    { key = "rapid10",   label = "Rapid 10m",  ms = 10 * 60 * 1000 },
    { key = "classic30", label = "Classic 30m", ms = 30 * 60 * 1000 },
},
DefaultTimeControl = "rapid10",
```

| Field | What it is |
|---|---|
| `key` | The internal id. Referenced by `DefaultTimeControl`. Do not change on an existing entry |
| `label` | What the button says |
| `ms` | Total time per side in **milliseconds**. `nil` means no clock |

Remove an entry to stop offering it. Add one for a control of your own, for example
`{ key = "bullet1", label = "Bullet 1m", ms = 60 * 1000 }`.

There is no increment or delay. Each side gets a total and it runs down on their turn.

## Bot

| Field | Default | What it does |
|---|---|---|
| `EloPresets` | `{ 600, 1000, 1400, 1800, 2200 }` | The strength buttons |
| `DefaultElo` | `1000` | Which one starts selected |
| `BotYieldEvery` | `800` | How much searching happens before the bot pauses to breathe |
| `BotMoveDelay` | `0.75` | Pause before it plays, so it does not feel instant |
| `BotLoadMinSeconds` | `0.75` | Minimum time the loading screen stays up |
| `BotLoadMaxSeconds` | `8` | Longest wait for the bot's avatar before starting anyway |

See [The bot](bot.md).

## Presentation

| Field | Default | What it does |
|---|---|---|
| `MoveTweenTime` | `0.45` | Seconds a piece takes to slide to its square |
| `ClockTickRate` | `0.25` | How often the clock display updates |
| `ResetDelay` | `8` | Seconds the result holds before the board resets and players stand |
| `Intro.Enabled` | `true` | The VS cut-scene |
| `Intro.HoldSeconds` | `5.1` | How long it holds |

## Feature switches

| Field | Default | What it does |
|---|---|---|
| `AllowDraws` | `true` | Players may offer and accept a draw |
| `AllowResign` | `true` | The resign button exists |
| `ConfirmResign` | `true` | Resigning asks first |
| `PromotionPicker` | `true` | The player chooses the promotion piece. Off promotes to a queen |
| `CameraLock` | `true` | Move the camera to the table's mount while seated |
| `ShowLastMove` | `true` | Highlight the previous move |
| `ShowCheck` | `true` | Highlight a king in check |

`CameraLock` needs a `Camera` folder in the table model. Without one, the setting does
nothing.

## Highlights

```lua
Highlights = {
    Selection = { 62, 166, 230 },
    LastMove = { 231, 159, 107 },
    Check = { 231, 76, 60 },
},
```

Red, green, blue from 0 to 255. Highlights are applied to the plate's tiles, which only
exist when the plate is a `Model` of per-square parts.

## The NPC

| Field | Default | What it does |
|---|---|---|
| `Npc.UseFriendAvatar` | `true` | Dress the bot as a random friend of the player |
| `Npc.FallbackUsername` | a specific account | Used when no friend is available |

Cosmetic. The game starts after `BotLoadMaxSeconds` whether or not the avatar arrived.

## Leaderstats and rating

| Field | Default | What it does |
|---|---|---|
| `Leaderstats.Enabled` | `true` | Create the player-list columns |
| `Wins`, `Losses`, `Draws`, `Elo` | those names | Column names. `false` stops tracking that one |
| `StartElo` | `1000` | Where a new player begins |
| `EloK` | `32` | How much a game moves a rating |
| `CountBotWinLoss` | `true` | Whether bot games move wins, losses and draws |
| `RatedVsBot` | `false` | Whether bot games move the rating |

Covered in [Rating and leaderboards](rating.md).

## Leaderboard

| Field | Default | What it does |
|---|---|---|
| `Enabled` | `true` | Run the in-world boards |
| `WorkspaceFolder` | `"uxrChessWorkspace"` | The world folder |
| `BoardsFolder` | `"Leaderboards"` | The folder of board models |
| `Stats` | `{ "Wins", "Losses", "Draws", "Elo" }` | One board each |
| `TopN` | `100` | Rows per board, capped at 100 |
| `RefreshSeconds` | `60` | How often global boards re-read |
| `Descending` | `true` | Highest first |
| `Store.Prefix`, `Store.Version` | `"uxrCH_LB_"`, `"v1"` | The ordered store name |

## Rewards

| Field | Default |
|---|---|
| `Rewards.Win` | `0` |
| `Rewards.Draw` | `0` |
| `Rewards.Loss` | `0` |
| `Rewards.PayBotGames` | `false` |

Paid into this system's profile balance. To pay in your own currency, leave these at zero
and use [Hooks](hooks.md).

## Sounds

```lua
Sounds = {
    Enabled = true,
    Volume = 0.5,
    Move = { "rbxassetid://…", "rbxassetid://…" },
    Capture = "",
    Check = "",
    GameEnd = "",
},
```

`Move` ships with seven real ids and plays one at random, which is what makes a game sound
like a game rather than a metronome. `Capture` and `Check` fall back to the move sound
when left empty, so every move is audible.

## Result messages

Nine strings on the end-of-game banner. Plain English, safe to translate.

| Key | Shown |
|---|---|
| `WinCheckmate`, `LossCheckmate` | Checkmate |
| `WinTime`, `LossTime` | A flag fell |
| `OpponentResigned`, `YouResigned` | A resignation |
| `DrawStalemate`, `Draw` | A drawn game |
| `GameOver` | The generic fallback |

## Profile

| Field | Default | What it does |
|---|---|---|
| `Profile.Store` | `"uxrCH_Profile_v1"` | The save slot. Changing it wipes balances, stats and ratings |
| `Profile.AutosaveSeconds` | `60` | Autosave period |
