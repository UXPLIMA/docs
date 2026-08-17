---
title: Stats and leaderboards
order: 5
description: The six tracked stats, the payout rules, and the in-world boards.
icon: trophy
---

## The six stats

```lua
Leaderstats = {
    Enabled = true,
    Wins = "Wins",
    Losses = "Losses",
    GamesPlayed = "Games",
    HighScore = "HighScore",
    Strikes = "Strikes",
    Spares = "Spares",
    StartValues = { Wins = 0, Losses = 0, Games = 0, HighScore = 0, Strikes = 0, Spares = 0 },
},
```

| Stat | Meaning |
|---|---|
| `Wins` | Games with the highest score |
| `Losses` | Games finished without winning |
| `GamesPlayed` | Every finished game |
| `HighScore` | Best single game, out of 300 |
| `Strikes` | Lifetime strikes |
| `Spares` | Lifetime spares |

The names on the right are the column titles in the player list. Rename them to fit your
game, or set `Enabled = false` to keep the list clean while still recording everything in
the profile.

`HighScore` is the interesting one for a leaderboard: it rewards a perfect game rather than
sheer volume, which is what a bowling alley should celebrate.

## Rewards

```lua
Rewards = { Win = 0, Play = 0, PayMultiplayerOnly = true },
```

| Field | Default | Paid |
|---|---|---|
| `Win` | `0` | To the winner |
| `Play` | `0` | To everyone who finished |
| `PayMultiplayerOnly` | `true` | Only pay when two or more players were in the game |

Both amounts are zero as shipped, so nothing is paid until you decide otherwise.

`PayMultiplayerOnly` is the anti-farming switch and it should stay on. Without it a player
can bowl alone, badly, for as long as they like and be paid for each game.

## The in-world boards

```lua
Leaderboard = {
    WorkspaceFolder = "uxrBowlingWorkspace",
    BoardsFolder = "Leaderboards",
    Stats = { "Wins", "Losses", "HighScore", "Strikes" },
    TopN = 25,
    RefreshSeconds = 60,
    Store = { Prefix = "uxrBW_LB", Version = 1 },
},
```

Build them in `Workspace/uxrBowlingWorkspace/Leaderboards`, one model per entry in `Stats`.

| Model name | Shows |
|---|---|
| `<Stat>Leaderboard` | The global top players across every server |
| `<Stat>ServerLeaderboard` | Only the players in this server |

Each board needs a `ListPart.ListGui.ScrollingFrame` holding a `Template` row with
`RankTextLabel`, `NameTextLabel` and `ValueTextLabel`. Everything else about the row is
your design.

`Stats` ships with four of the six. Add `Games` or `Spares` if you want boards for them;
each entry needs its own board model.

Bumping `Store.Version` starts a fresh season on every board at once without touching
profiles.

<Callout type="warning" title="Global boards stay empty in Studio">

They read ordered stores that only the live game writes to. Test your row layout against a
server board, which is built from connected players and works anywhere.

</Callout>

## The lane monitor

If the lane model has a `Lane<i>Monitor` part, the scorecard is drawn on it: the frames,
the running total and whose turn it is.

It is optional. Without it players read the scorecard from their own interface, which every
player has anyway. The monitor is what makes a lane feel like a lane to somebody walking
past.

## Where progress is saved

```lua
Profile = { Store = "uxrBW_Profile_v1", AutosaveSeconds = 60 },
```

One profile per player holding the six stats and the balance. Changing `Store` gives
everyone a fresh profile, which is the deliberate way to wipe after a big change.

That is separate from `Leaderboard.Store.Version`, which resets only the boards.
