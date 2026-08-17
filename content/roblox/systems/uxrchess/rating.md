---
title: Rating and leaderboards
order: 5
description: The ELO formula, the K factor, and the in-world boards.
icon: trophy
---

Four numbers per player: wins, losses, draws and a rating. They show in the player list,
save with the profile, and drive the in-world boards.

## The rating

Standard ELO. After a game each player's rating moves by

```
K * (score - expected)
```

where `score` is 1 for a win, 0.5 for a draw and 0 for a loss, and `expected` is the usual
logistic function of the rating difference.

| Field | Default | Effect |
|---|---|---|
| `Leaderstats.StartElo` | `1000` | Where everyone begins |
| `Leaderstats.EloK` | `32` | How much one game can move a rating |
| `Leaderstats.RatedVsBot` | `false` | Whether bot games are rated at all |

`EloK = 32` is the classic value: a player rated equally with their opponent gains 16 for
a win. Raise it for a game where players play a handful of matches and want to see
movement; lower it, 16 or 24, if you want a stable ladder over hundreds of games.

Ratings are rounded to whole numbers on write.

## Rated bot games

With `RatedVsBot = true`, a game against the bot is rated against the **preset number** as
the bot's rating. Beating the 2200 button moves a player a lot; beating the 600 button
barely moves them, and losing to it costs.

That is mathematically consistent, and it is still farmable: a player can grind draws
against a bot they know they can hold. The shipped `false` avoids the question entirely.

## Turning a stat off

```lua
Leaderstats = {
    Elo = false,
},
```

Setting a stat's name to `false` stops tracking it. `Elo = false` gives you a chess system
with wins, losses and draws and no ladder, which suits a casual game.

## The in-world boards

Build them in the world folder:

```
Workspace/uxrChessWorkspace/Leaderboards/
  EloLeaderboard
  EloServerLeaderboard
  WinsLeaderboard
  …
```

One model per entry in `Leaderboard.Stats`, and the naming decides the scope:

| Model name | Shows |
|---|---|
| `<Stat>Leaderboard` | The global top players across every server |
| `<Stat>ServerLeaderboard` | Only the players in this server |

Each needs:

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

The three labels are filled in. The rest of the row is your design.

## Global versus server

| | Source | Refresh |
|---|---|---|
| `<Stat>Leaderboard` | An `OrderedDataStore` | Every `RefreshSeconds`, default 60 |
| `<Stat>ServerLeaderboard` | Connected players | The same tick |

The store name is `Store.Prefix .. Stat .. Store.Version`: with the shipped values the ELO
board reads `uxrCH_LB_Elov1`.

Bumping `Store.Version` starts a fresh board without touching profiles. That is how you
run a season.

<Callout type="warning" title="Global boards stay empty in Studio">

They read an ordered store only the live game writes to, so a Studio session shows nothing.
That is correct, not broken.

Check your row layout against a server board instead, which is built from connected
players and works anywhere.

</Callout>

## Rewards

```lua
Rewards = { Win = 0, Draw = 0, Loss = 0, PayBotGames = false },
```

Paid into this system's profile balance. All zero by default. To pay in your own currency
instead, leave them at zero and use [Hooks](hooks.md), so one system stays in charge of
your economy.

## What lives where

| Place | Holds | Reset by |
|---|---|---|
| `leaderstats` | The four numbers, live | Nothing, rebuilt from the profile |
| The profile | Those four plus the balance | Changing `Profile.Store` |
| The ordered stores | Top 100 per stat | Bumping `Store.Version` |
