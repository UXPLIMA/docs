---
title: Setup
order: 1
description: Install, build one table, and confirm the orientation calibration passed.
icon: play
---

Install with the standard four steps in
[Installing a system](../platform/installation.md): drag `uxrChessSystem` into
**ServerScriptService** and press Play once.

This system needs a table built in the world before it does anything.

## 1. Build the world folder

```
Workspace/
  uxrChessWorkspace/
    Stands/
```

Both names are fixed by `Settings.Leaderboard.WorkspaceFolder` and the `Stands`
convention.

## 2. Put one table in it

The full contract is in [Building a table](table.md). In short: a model with a `Board`, a
square `Plate`, `White` and `Black` folders holding correctly named pieces **in the
standard starting position**, and two chairs each containing a `Seat`.

Tag the model `ChessTable`.

## 3. Press Play and read Output

This is the step that matters. If the pieces are not set up exactly, the table refuses to
load and says why:

| Output | Means |
|---|---|
| `Stands folder not found` | The world folder is missing or misnamed |
| `plate (Plate / FullPlate) not found` | The board has no plate part |
| `could not calibrate board orientation` | The pieces are not in the standard start position |
| `white King not found` | No part named `King` under `White`. `Chair1` defaults to white |

Nothing is wrong with your code when these appear. They are all model problems, and all of
them are quick to fix.

## 4. Sit down and play the bot

Sit in a chair. The seat panel offers a time control and a bot strength. Start a game and
check three things:

- pieces slide to the right squares
- the highlight lands on the square you selected
- captured pieces stack at the edge rather than vanishing

## 5. Play a human game

Sit at one chair, have a second player or a second Studio client sit at the other. The
game starts on its own; there is no ready button.

Watch the clocks. `DefaultTimeControl` ships as `rapid10`, ten minutes each.

## 6. Set the numbers you care about

| Setting | Default | Worth reviewing |
|---|---|---|
| `Debug` | `true` | Turn it off before release |
| `TimeControls` | four entries | Drop the ones you do not want offered |
| `EloPresets` | `{ 600, 1000, 1400, 1800, 2200 }` | The bot strength buttons |
| `Leaderstats.RatedVsBot` | `false` | Whether bot games move a player's rating |
| `Npc.FallbackUsername` | a specific account | Point it at one of yours |
| `Rewards` | all `0` | Pays nothing until set |

<Callout type="tip" title="Debug ships on for this system">

`Settings.Debug` defaults to `true` here, unlike most of the range. That is useful while
you are getting a table to load and noise afterwards. Turn it off when you publish.

</Callout>

## 7. Publish

Everything is in `Shared/Config/Settings.luau`. The global leaderboards only accumulate in
the live game, so expect them empty in Studio.
