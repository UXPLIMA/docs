---
title: Stats and leaderboards
order: 6
description: The four tracked stats, why 180s and nine-darters are separate, and the boards.
icon: trophy
---

## The four stats

```lua
Leaderstats = {
    Enabled = true,
    Wins = "Wins",
    Elo = "Elo",
    Eighties = "180s",
    NineDarters = "9-Darters",
    StartElo = 1000,
},
```

| Stat | Meaning |
|---|---|
| `Wins` | Matches won |
| `Elo` | Rating, starting at `StartElo` |
| `Eighties` | Visits of three treble 20s: a maximum 180 |
| `NineDarters` | 501 finished in nine darts |

Set any name to `false` to stop showing that column, or `Enabled = false` for none.

## Why 180s and nine-darters are tracked separately

Wins measure how often you beat somebody. A rating measures who you beat. Neither says
anything about how well you threw.

A 180 is the maximum visit, and a nine-darter is the shortest possible 501 leg. They are the
two moments in darts that a player will tell somebody about, and they are worth their own
counters precisely because they are rare.

They are also excellent leaderboard columns: a player can rank on 180s without grinding
matches.

<Callout type="warning" title="Widening the treble ring devalues both stats">

`Config.Radii.TrebleIn` and `TrebleOut` decide how wide the treble band is. Widening it
makes the game friendlier and makes 180s common.

If you do it, expect the 180s board to stop meaning anything within a week.

</Callout>

## The in-world boards

```lua
Config.Leaderboard = {
    Enabled = true,
    WorkspaceFolder = "uxrDartsWorkspace",
    BoardsFolder = "Leaderboards",
    Stats = { "Wins", "Elo", "Eighties", "NineDarters" },
    Descending = true,
    TopN = 25,
    RefreshSeconds = 60,
    Store = { Prefix = "uxrDS_LB_", Version = "v1" },
    StartElo = 1000,
},
```

Build them in `Workspace/uxrDartsWorkspace/Leaderboards`, one model per entry in `Stats`.

| Model name | Shows |
|---|---|
| `<Stat>Leaderboard` | The global top players across every server |
| `<Stat>ServerLeaderboard` | Only the players in this server |

Each needs a `ListPart.ListGui.ScrollingFrame` containing a `Template` row with
`RankTextLabel`, `NameTextLabel` and `ValueTextLabel`.

Bumping `Store.Version` starts a fresh season without touching profiles.

## Two places set StartElo

`Settings.Leaderstats.StartElo` and `Config.Leaderboard.StartElo` both default to `1000`.

Keep them the same. They are the starting rating as the player list shows it and as the
board ranks it, and a mismatch makes an unrated player appear at a different position
depending on where you look.

## Accuracy is stored too

Alongside the four visible stats the profile keeps `DartsThrown` and `DartsHit`. They are
not shown anywhere, and they drive the throw model: a player's accuracy is their hit rate.

See [Throwing](throwing.md). If you reset profiles, players lose their accumulated accuracy
along with their stats and go back to throwing at the default.
