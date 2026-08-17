---
title: Building a table
order: 2
description: The world folder, the parts a table model needs, and how cells are measured.
icon: box
---

The tables are yours. The bootstrap does not create a world folder or spawn any furniture:
it looks for a folder you built and adopts whatever is in it.

## The world folder

```
Workspace/
  uxrTicTacToeWorkspace/
    Stands/                 one model per table
    Leaderboards/           optional, see Stats and leaderboards
```

Anything in `Stands` that contains a `Board` child is treated as a table. Tables added
while the server is running are picked up too, so a table spawned by your own code works
without a restart.

If the folder is missing, Output says `[uxrTTT] Stands folder not found; no tables` and
nothing else happens.

## One table

```
<any name>                        tagged TicTacToeTable
  Board
    Plate                         a square BasePart, or a Model
    Cells/                        optional tile parts, one per physical cell
    Marks/
      X                           template cloned onto a cell
      O                           template cloned onto a cell
  Chair1                          contains a Seat, plays X
  Chair2                          contains a Seat, plays O
  Camera/                         optional invisible mount parts
```

| Instance | Required | Notes |
|---|---|---|
| `Board.Plate` | Yes | Defines the whole geometry. Missing means the table is skipped |
| `Board.Marks.X` and `.O` | Yes | Cloned per move. Parts or models |
| `Chair1`, `Chair2` | Yes | Each must contain a `Seat` at any depth |
| `Board.Cells` | No | Tile parts that light up. Without them the board still works |
| `Camera` | No | Mount parts for the seated camera |

A table missing a required piece warns once and is left alone. The rest of the tables in
the folder still work.

## Plate size decides everything

There is no calibration step and no offset attribute. The system measures the plate:

```
one cell = Plate.Size.X / Settings.PhysicalGrid
```

`PhysicalGrid` ships as `5`. So a plate must be square and sized as five cells across,
whatever a cell is in studs for your model.

A 3x3 game is played on the **centred sub-grid** of that 5x5 physical board, and the outer
ring of cells is hidden. That is why one physical table can host all three board sizes
without rebuilding anything.

<Callout type="danger" title="A non-square plate puts marks in the wrong places">

The cell size comes from the X dimension only. If the plate is 20 by 16 studs, columns are
measured from the 20 and rows use the same number, so marks drift further off with every
row.

Build the plate square. If it is a `Model`, its bounding box is used, so keep decorative
overhang out of it.

</Callout>

## Cells and their transparency

If a `Cells` folder is present, its parts are used for hover and win-line highlighting.
Give a tile that is not fully opaque a `BaseTransparency` attribute holding its resting
transparency, and the system restores that value rather than forcing it to zero.

Cells outside the current board size are hidden when the size changes, and shown again
when it grows.

## Marks

`Marks.X` and `Marks.O` are templates. They are cloned into a `Placed` folder the system
creates inside the table model, and that folder is cleared between games.

Do not put your own instances in `Placed`. It is rebuilt on start and emptied on reset.

## Chairs

`Chair1` is X and moves first. `Chair2` is O. Each needs a `Seat` somewhere inside it; a
seat directly in the model works, and so does a seat inside a chair model you bought.

Sitting in a chair claims that side. Standing up mid-game resigns it.

## The tag

Table models are tagged `TicTacToeTable`, configurable as `Settings.Tag`. The tag is how
your own tooling can find them; discovery at startup is by folder membership, so an
untagged model in `Stands` with a `Board` still works.

## Placing several tables

Copy the model. Each is independent: its own board size, its own game, its own seats.
There is no global limit and nothing shared between them except the leaderboards.
