---
title: Settings
order: 6
description: Every field in Settings.luau with its default.
icon: sliders-horizontal
---

`Shared/Config/Settings.luau`. One file for the whole system.

## System and table

| Field | Default | What it does |
|---|---|---|
| `Debug` | `false` | Extra `[uxrCC]` lines in Output |
| `AssetFolder` | `"Assets"` | Designer models folder under `ServerStorage/<package>` |
| `WorkspaceFolder` | `"uxrColorCardsWorkspace"` | The world folder |
| `TablesFolder` | `"Tables"` | The folder of table models inside it |
| `Seats` | `4` | Chairs looked for per table |
| `MinPlayers` | `2` | Fewest players to start |
| `StartingHand` | `7` | Cards dealt to each player |

## Colours

```lua
Colors = {
    Red = { 226, 42, 42 },
    Yellow = { 247, 197, 22 },
    Green = { 46, 176, 74 },
    Blue = { 32, 116, 232 },
    Wild = { 22, 22, 28 },
},
```

Red, green and blue from 0 to 255. Used by the interface, the turn sign and the colour
picker.

## House rules

| Field | Default | Implemented |
|---|---|---|
| `Stacking` | `true` | Yes |
| `LastCallout` | `true` | Yes |
| `DrawUntilPlayable` | `false` | Yes |
| `StrictWildFour` | `false` | Yes |
| `SevenZero` | `false` | **No** |
| `JumpIn` | `false` | **No** |

See [House rules](house-rules.md).

## Timing

| Field | Default | What it does |
|---|---|---|
| `MoveSeconds` | `25` | Seconds per turn |
| `ClockTickRate` | `0.25` | How often the countdown updates |
| `BotMoveDelay` | `0.9` | Pause before a bot plays |
| `LastWindowSeconds` | `3` | Time to call last card |
| `ResetDelay` | `6` | Seconds the result holds before the table clears |
| `BotLoadMinSeconds` | `0.5` | Minimum loading splash |
| `BotLoadMaxSeconds` | `8` | Longest wait for a bot avatar |

## Bot

| Field | Default |
|---|---|
| `Difficulties.Easy` | `{ randomness = 0.5, callsLast = false }` |
| `Difficulties.Medium` | `{ randomness = 0.15, callsLast = true }` |
| `Difficulties.Hard` | `{ randomness = 0, callsLast = true }` |
| `DefaultDifficulty` | `"Medium"` |
| `UseFriendAvatar` | `true` |
| `FallbackUsernames` | Twelve well-known accounts |

See [Bots](bots.md).

## Leaderstats

| Field | Default | What it does |
|---|---|---|
| `Enabled` | `true` | Create the player-list columns |
| `Wins`, `Losses`, `Points` | those names | Column names |
| `CountBotGames` | `true` | Whether bot games count |
| `StartValues` | all `0` | Where a new player starts |

## Leaderboard

| Field | Default |
|---|---|
| `Enabled` | `true` |
| `WorkspaceFolder` | `"uxrColorCardsWorkspace"` |
| `BoardsFolder` | `"Leaderboards"` |
| `Stats` | `{ "Wins", "Points" }` |
| `TopN` | `100` |
| `RefreshSeconds` | `60` |
| `Descending` | `true` |
| `Store.Prefix`, `Store.Version` | `"uxrCC_LB_"`, `"v1"` |

Build one model per entry in `Stats` under
`Workspace/uxrColorCardsWorkspace/Leaderboards`, named `<Stat>Leaderboard` for the global
board or `<Stat>ServerLeaderboard` for this server. Each needs a
`ListPart.ListGui.ScrollingFrame` with a `Template` row holding `RankTextLabel`,
`NameTextLabel` and `ValueTextLabel`.

Bumping `Store.Version` starts a fresh season without touching profiles.

## Intro

| Field | Default |
|---|---|
| `Intro.Enabled` | `true` |
| `Intro.HoldSeconds` | `2.5` |

## Sounds

`Sounds.Enabled` is the master switch. `Sounds.Events` holds seventeen entries, each
`{ Id = "", Volume = n }`:

| Event | Volume | Plays |
|---|---|---|
| `CardPlay`, `CardDraw` | `0.5` | Cards |
| `YourTurn` | `0.5` | Your turn begins |
| `Skip`, `Reverse`, `DrawTwo`, `DrawFour`, `Wild` | `0.5` | Action cards |
| `LastCall`, `Penalty` | `0.6` | The last-card moment |
| `Win`, `Lose` | `0.6` | The result |
| `GameStart` | `0.5` | A game beginning |
| `ClockLow` | `0.5` | The turn timer running out |
| `UIClick`, `Notify` | `0.4` | Interface |
| `Music` | `0.3` | Background |

Every `Id` ships empty, so the system is silent until you paste ids in. The volumes are
already balanced: effects at 0.5, the dramatic moments at 0.6, interface at 0.4 and music
at 0.3.

## Card images

`CardImages` maps 63 card keys to asset ids. See [Building a table](table.md) for the key
format.

## Profile

| Field | Default |
|---|---|
| `Profile.Store` | `"uxrCC_Profile_v1"` |
| `Profile.AutosaveSeconds` | `60` |

## Messages

| Key | Notes |
|---|---|
| `WaitingPlayers` | Shown at an unfilled table |
| `PlayButton` | The play button label |
| `YourTurn`, `TheirTurn` | `TheirTurn` takes the player's name via `%s` |
| `LastCard` | The last-card call |
| `ChooseColor` | The wild colour picker |
| `NowPlaying` | The turn sign header |

`ResultMessages` holds `Win`, `Loss`, `GameOver`, `PlayerLeft` (which takes a name via
`%s`) and `LastPenalty`.

Keep the `%s` in the two strings that have one. Removing it does not error; it just stops
naming the player.
