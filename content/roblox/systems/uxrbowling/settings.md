---
title: Settings
order: 6
description: Every field in Settings.luau with its default.
icon: sliders-horizontal
---

`Shared/Config/Settings.luau`. One file for the whole system.

## System

| Field | Default | What it does |
|---|---|---|
| `Debug` | `false` | Extra `[uxrBW]` lines in Output. Turn it on while registering lanes |
| `Currency` | `"$"` | Symbol beside amounts |
| `StartingBalance` | `0` | Balance a new player begins with |

## Lane

| Field | Default | What it does |
|---|---|---|
| `Lane.Tag` | `"BowlingLane"` | Internal name. Leave it |
| `LobbyCountdownSeconds` | `10` | Wait after the first player joins |
| `MinPlayers` | `1` | Fewest to start. `1` allows solo |
| `MaxPlayers` | `4` | Most per lane. Also fills the wait spots |
| `TurnSeconds` | `30` | Time to throw before a gutter ball is thrown for you |
| `ResultsDisplaySeconds` | `6` | How long the final scoreboard holds. `0` clears instantly |

## Game

| Field | Default | What it does |
|---|---|---|
| `Game.Frames` | `10` | Frames per player |
| `Game.PinCount` | `10` | Pins per rack |

Standard values. Change only for a deliberately non-standard game.

## Throw

| Field | Default | What it does |
|---|---|---|
| `MaxAngleDeg` | `45` | Widest aim either side |
| `DirectionHz` | `0.34` | Speed of the direction meter. Higher is harder |
| `PowerHz` | `0.46` | Speed of the power meter |
| `SpinHz` | `0.41` | Speed of the spin meter |
| `PowerMin` | `28` | Ball speed at the lightest tap |
| `PowerMax` | `60` | Ball speed at full power |
| `SpinMax` | `1.0` | Maximum hook. `0` is none |
| `StrafeRange` | `2.5` | How far the bowler may step sideways |
| `BallDensity` | `5` | Ball weight |
| `BallFriction` | `0.35` | Grip on the lane |
| `BallElasticity` | `0.05` | Bounce. Keep near zero |
| `PinDensity` | `3` | Pin weight |
| `PinFriction` | `0.4` | How far pins slide |
| `PinElasticity` | `0.05` | Keep near zero |
| `SettleSeconds` | `2.5` | Wait before counting pins |
| `PinUprightDot` | `0.85` | How upright a pin must be to count as standing |
| `PinMoveThreshold` | `1.5` | How far it may have moved and still count |
| `BallSpawnOffset` | `Vector3.new(0, 0, 0)` | Nudge for the ball's spawn point |
| `CurveForce` | `40` | How hard the hook bends |
| `CurveDurationSeconds` | `1.2` | How long the hook is applied |

See [The throw](throw.md). The camera is not here: it follows the camera mount part in the
lane model.

## Machine

| Field | Default | What it does |
|---|---|---|
| `SetterY` | `Vector3.new(0, 2.583, 0)` | Pinsetter travel, up and down |
| `SweepY` | `Vector3.new(0, 3.87, 0)` | Sweep bar travel, up and down |
| `SweepX` | `Vector3.new(12, 0, 0)` | Sweep bar travel, across the lane |
| `StepSeconds` | `0.5` | Seconds per animation step |
| `EasingStyle` | `Quad` | |
| `EasingDirection` | `InOut` | |
| `SettlePause` | `0.15` | Pause at the bottom of a move so the swap reads |
| `ReturnSeconds` | `1.2` | Ball return travel time |
| `ReturnArcHeight` | `2.0` | How high the returning ball arcs |

These match the shipped lane model. A differently sized pinsetter needs different numbers.

## Bumper

| Field | Default | What it does |
|---|---|---|
| `Bumper.DefaultOn` | `false` | Whether bumpers start up. Players toggle it in the lobby |

## Rewards

| Field | Default | What it does |
|---|---|---|
| `Rewards.Win` | `0` | Paid to the winner |
| `Rewards.Play` | `0` | Paid to everyone who finished |
| `PayMultiplayerOnly` | `true` | Only pay games with two or more players |

## Leaderstats

| Field | Default |
|---|---|
| `Enabled` | `true` |
| `Wins`, `Losses` | `"Wins"`, `"Losses"` |
| `GamesPlayed` | `"Games"` |
| `HighScore` | `"HighScore"` |
| `Strikes`, `Spares` | `"Strikes"`, `"Spares"` |
| `StartValues` | all `0` |

## Leaderboard

| Field | Default |
|---|---|
| `WorkspaceFolder` | `"uxrBowlingWorkspace"` |
| `BoardsFolder` | `"Leaderboards"` |
| `Stats` | `{ "Wins", "Losses", "HighScore", "Strikes" }` |
| `TopN` | `25` |
| `RefreshSeconds` | `60` |
| `Store.Prefix` | `"uxrBW_LB"` |
| `Store.Version` | `1` |

See [Stats and leaderboards](stats.md).

## Replay

| Field | Default | What it does |
|---|---|---|
| `Replay.Enabled` | `true` | Play slow-motion replays |
| `MaxSeconds` | `5` | Longest a replay runs |
| `HoldSeconds` | `7` | Wait before the next turn. Keep it above `MaxSeconds` |

## Sounds

| Key | Ships | Plays |
|---|---|---|
| `BallRoll` | A real id | The ball down the lane |
| `PinHit` | A real id | The ball hitting the pins |
| `Strike` | A real id | A strike |
| `ReplayMusic` | A real id | Under the replay |
| `Spare` | Silent | A spare |
| `GutterBall` | Silent | A gutter ball |
| `Ambience` | Silent | Looping background |
| `UIClick` | Silent | Interface buttons |

`rbxassetid://0` means silent. `Sounds.Volume` holds a per-sound level from 0 to 1;
`ReplayMusic` and `Ambience` ship at `0.5` so they sit under the effects.

## Profile

| Field | Default | What it does |
|---|---|---|
| `Profile.Store` | `"uxrBW_Profile_v1"` | The save slot. Changing it wipes everyone |
| `Profile.AutosaveSeconds` | `60` | Autosave period |

## Messages

| Key | Default |
|---|---|
| `GameInProgress` | `"A game is already in progress on this lane."` |
| `LaneFull` | `"This lane is full (4 players)."` |
| `Joined` | `"Joined! Game starts soon."` |

`LaneFull` names four players in its text. If you change `MaxPlayers`, change this string
too: nothing substitutes the number for you.
