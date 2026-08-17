---
title: Setup
order: 1
description: Install, build one table, and play a game against the bot.
icon: play
---

Install with the standard four steps in
[Installing a system](../platform/installation.md): drag `uxrTicTacToeSystem` into
**ServerScriptService** and press Play once.

Unlike most systems, this one needs something built in the world before it does anything.

## 1. Build the world folder

```
Workspace/
  uxrTicTacToeWorkspace/
    Stands/
```

Both names matter. `uxrTicTacToeWorkspace` is fixed; `Stands` is where table models go.

## 2. Put one table in it

The full contract is in [Building a table](table.md). The short version:

- a model containing `Board`, `Chair1` and `Chair2`
- `Board.Plate`, a square part sized five cells across
- `Board.Marks.X` and `Board.Marks.O`, the templates cloned onto cells
- a `Seat` inside each chair

Tag the model `TicTacToeTable`.

## 3. Press Play and sit down

Sit in `Chair1`. The seat panel appears with the board sizes, the bot difficulties and a
start button.

Press start. The VS intro plays, the bot's avatar appears in the other chair, and you move
first as X.

## 4. Check the geometry

Play a full game and watch where the marks land. They should sit centred on their cells.
If they drift, the plate is not square or is not sized as `PhysicalGrid` cells across.
That is the one thing worth getting right before building ten more tables.

## 5. Turn the leaderboards on

They ship enabled but need boards built in `Workspace/uxrTicTacToeWorkspace/Leaderboards`.
Until then they simply do nothing. See [Stats and leaderboards](stats.md).

## 6. Set the numbers you care about

| Setting | Default | Worth reviewing |
|---|---|---|
| `BoardSizes` | `{ 3, 4, 5 }` | Cut it to `{ 3 }` for a classic-only table |
| `MoveSeconds` | `20` | `nil` removes the timer entirely |
| `DefaultDifficulty` | `"Medium"` | `Impossible` is unbeatable on 3x3 |
| `Rewards` | all `0` | Pays nothing until you set amounts |
| `Npc.FallbackUsername` | a real account | Change it to one of yours |

<Callout type="tip" title="The shipped NPC falls back to a specific username">

`Npc.FallbackUsername` ships as `siracozmen01`, the developer's account. It is only used
when the friend-avatar lookup finds nothing, but that is common enough to notice. Point it
at an account you control.

</Callout>

## 7. Publish

Everything is in `Shared/Config/Settings.luau`, so publishing the place ships the
configuration. Leaderboards need the live game to accumulate anything: their store is
global, and Studio sessions do not write to it usefully.
