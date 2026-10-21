---
title: Stats and leaderboards
order: 5
description: The three counters, the in-world boards, and what is global versus per server.
icon: trophy
---

Three numbers per player: wins, losses, draws. They appear in the Roblox player list, they
are saved with the profile, and they feed the in-world boards.

There is no rating or ELO. That is a design decision, not an omission: a rating system
needs matchmaking to mean anything, and these are walk-up tables.

## leaderstats

```lua
Leaderstats = {
    Enabled = true,
    Wins = "Wins",
    Losses = "Losses",
    Draws = "Draws",
    CountBotGames = true,
    StartValues = { Wins = 0, Losses = 0, Draws = 0 },
},
```

The three names are the column names in the player list. Rename them to fit your game, or
set `Enabled = false` to keep the list clean and still record the stats in the profile.

If your game already creates a `leaderstats` folder, this system adds its columns to it
rather than replacing it.

## The in-world boards

Build them yourself in the world folder:

```
Workspace/uxrTicTacToeWorkspace/Leaderboards/
  WinsLeaderboard
  WinsServerLeaderboard
  LossesLeaderboard
  DrawsLeaderboard
```

One model per entry in `Leaderboard.Stats`. Naming decides what a board shows:

| Model name | Shows |
|---|---|
| `<Stat>Leaderboard` | The global top players across every server |
| `<Stat>ServerLeaderboard` | The players in this server only |

Each board model needs:

```
<Stat>Leaderboard
  ListPart
    ListGui                     a SurfaceGui
      ScrollingFrame
        Template                one row, cloned per entry
          RankTextLabel
          NameTextLabel
          ValueTextLabel
```

The three labels are filled in per row. Everything else about the row is yours.

## Global versus server

| | Source | Freshness |
|---|---|---|
| `<Stat>Leaderboard` | An `OrderedDataStore` | Re-read every `RefreshSeconds` |
| `<Stat>ServerLeaderboard` | The players currently connected | Rebuilt on the same tick |

The global store name is `Store.Prefix .. Stat .. Store.Version`, so with the shipped
values the wins board reads `uxrTTT_LB_Winsv1`.

Bumping `Store.Version` starts a fresh board without touching player profiles. That is the
clean way to reset a leaderboard for a new season.

| Field | Default | Effect |
|---|---|---|
| `TopN` | `100` | Rows kept in the store |
| `RefreshSeconds` | `60` | Seconds between reads |
| `Descending` | `true` | Highest first. `false` makes it a bottom table |

<Callout type="warning" title="Leaderboards do not fill up in Studio">

The global boards read an `OrderedDataStore` that only your live game writes to. A Studio
session shows empty boards, which is correct rather than broken.

Test the layout by checking a server board, which is built from connected players and
works anywhere.

</Callout>

## Rewards

```lua
Rewards = {
    Win = 0,
    Draw = 0,
    Loss = 0,
    PayBotGames = false,
},
```

Paid into the balance this system's profile keeps. All three ship at zero, so nothing is
paid until you decide otherwise.

To pay in your own currency, leave them at zero and do it from
[Hooks](hooks.md). That keeps one system in charge of your economy.

## Where the numbers are kept

| Place | Holds | Lifetime |
|---|---|---|
| `leaderstats` | The three counters, live | The session |
| The profile | The same three plus the balance | Saved to `Profile.Store` |
| The ordered store | Top `TopN` per stat | Global, keyed by `Store.Version` |

Changing `Profile.Store` wipes stats and balances. Changing `Store.Version` wipes only the
boards. They are separate on purpose.
