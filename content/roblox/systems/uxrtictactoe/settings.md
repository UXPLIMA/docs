---
title: Settings
order: 3
description: Board sizes, timers, rewards, sounds and messages, with defaults.
icon: sliders-horizontal
---

`Shared/Config/Settings.luau`.

## Identity and world

| Field | Default | What it does |
|---|---|---|
| `Debug` | `false` | Extra `[uxrTTT]` lines in Output |
| `Currency` | `"$"` | Symbol beside amounts in the interface |
| `StartingBalance` | `0` | Balance a new profile begins with |
| `AssetFolder` | `"Assets"` | Designer models folder under `ServerStorage/<package>` |
| `Tag` | `"TicTacToeTable"` | The tag your table models carry |
| `PhysicalGrid` | `5` | Cells across the physical plate. See [Building a table](table.md) |

## Board sizes

| Field | Default | What it does |
|---|---|---|
| `BoardSizes` | `{ 3, 4, 5 }` | Sizes offered on the seat panel |
| `WinLengths` | `{ [3] = 3, [4] = 4, [5] = 4 }` | Marks in a row needed to win, per size |
| `DefaultSize` | `3` | Which size a free table starts on |

`WinLengths` must have an entry for every size in `BoardSizes`. Five in a row on a 5x5
board is possible but nearly always a draw, which is why the shipped value is four.

## Timing

| Field | Default | What it does |
|---|---|---|
| `MoveSeconds` | `20` | Seconds per move. `nil` disables the timer |
| `OnTimeout` | `"automove"` | `"automove"` plays a random empty cell, `"loseturn"` passes |
| `ClockTickRate` | `0.25` | How often the countdown updates |
| `BotMoveDelay` | `0.6` | Pause before the bot replies, so it does not feel instant |
| `ResetDelay` | `6` | Seconds the result stays up before the board clears |

`OnTimeout = "loseturn"` on a two-player game can loop: if neither player ever moves, turns
pass back and forth until somebody stands up. `"automove"` always terminates.

## Bot

| Field | Default | What it does |
|---|---|---|
| `Difficulties` | Easy, Medium, Impossible | Search depth and randomness per level |
| `DefaultDifficulty` | `"Medium"` | Which button starts selected |
| `BotMaxThinkSeconds` | `1.0` | Hard cap on thinking time per move |
| `BotLoadMinSeconds` | `0.5` | Minimum time the loading splash stays up |
| `BotLoadMaxSeconds` | `8` | Longest wait for the bot avatar before starting anyway |

Covered in [The bot](bot.md).

## The NPC

| Field | Default | What it does |
|---|---|---|
| `Npc.UseFriendAvatar` | `true` | Dress the bot as a random friend of the player |
| `Npc.FallbackUsername` | a specific account | Used when no friend is available |

The avatar is cosmetic. A slow or failed load never blocks the game; after
`BotLoadMaxSeconds` the game starts regardless.

## The intro

| Field | Default | What it does |
|---|---|---|
| `Intro.Enabled` | `true` | Show the VS cut-scene when a game starts |
| `Intro.HoldSeconds` | `3` | How long it holds |

## Leaderstats

| Field | Default | What it does |
|---|---|---|
| `Leaderstats.Enabled` | `true` | Create the player-list columns |
| `Wins`, `Losses`, `Draws` | those names | Column names. Rename to match your game |
| `CountBotGames` | `true` | Whether bot games move these numbers |
| `StartValues` | all `0` | Where a new player starts |

There is no ELO or rating. Three counters, deliberately.

Setting `CountBotGames = false` is the honest choice if you also run a leaderboard: a
player can farm an unlimited number of wins against Easy.

## Leaderboard

| Field | Default | What it does |
|---|---|---|
| `Leaderboard.Enabled` | `true` | Run the in-world boards |
| `WorkspaceFolder` | `"uxrTicTacToeWorkspace"` | The world folder |
| `BoardsFolder` | `"Leaderboards"` | The folder of board models inside it |
| `Stats` | `{ "Wins", "Losses", "Draws" }` | One board per entry |
| `TopN` | `100` | How many rows are kept |
| `RefreshSeconds` | `60` | How often boards re-read the store |
| `Descending` | `true` | Highest first |
| `Store.Prefix`, `Store.Version` | `"uxrTTT_LB_"`, `"v1"` | The ordered store name |

See [Stats and leaderboards](stats.md).

## Rewards

| Field | Default | What it does |
|---|---|---|
| `Rewards.Win` | `0` | Currency paid to the winner |
| `Rewards.Draw` | `0` | Paid to both on a draw |
| `Rewards.Loss` | `0` | A consolation payout |
| `Rewards.PayBotGames` | `false` | Whether bot games pay at all |

All zero means nothing is paid, which is the shipped state. Payouts go to the profile
balance this system keeps. To pay in your own currency instead, leave these at zero and
use [Hooks](hooks.md).

## Highlights

```lua
Highlights = {
    WinLine = { 95, 220, 130 },
    LastMove = { 231, 159, 107 },
},
```

Colours as red, green, blue from 0 to 255.

## Sounds

| Field | Default | Plays |
|---|---|---|
| `Sounds.Enabled` | `true` | Master switch |
| `Volume` | `0.5` | |
| `Move` | `""` | A mark being placed |
| `Win` | `""` | A win |
| `Draw` | `""` | A draw |
| `UIClick` | `""` | Interface buttons |

Each accepts one asset id, or a list of ids to pick from at random.

## Profile

| Field | Default | What it does |
|---|---|---|
| `Profile.Store` | `"uxrTTT_Profile_v1"` | The save slot. Changing it wipes everyone |
| `Profile.AutosaveSeconds` | `60` | Autosave period |

## Messages

`Messages` holds the three in-play strings, `ResultMessages` the five end-of-game ones.
Both are plain English text and safe to translate.

| Key | Shown |
|---|---|
| `Messages.WaitingOpponent` | Seated, nobody opposite |
| `Messages.YourTurn`, `TheirTurn` | During play |
| `ResultMessages.Win`, `Loss`, `Draw` | On the result banner |
| `ResultMessages.OpponentResigned` | The other player stood up or left |
| `ResultMessages.GameOver` | The generic fallback |
