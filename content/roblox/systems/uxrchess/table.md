---
title: Building a table
order: 2
description: The world folder, the piece naming, and the automatic orientation calibration.
icon: box
---

Tables are yours to build. Nothing is spawned for you: the system looks for a folder you
made and adopts what is in it.

## The world folder

```
Workspace/
  uxrChessWorkspace/
    Stands/                 one model per table
    Leaderboards/           optional
```

Anything in `Stands` with a `Board` child is treated as a table, including one added while
the server is running. A missing folder logs `[uxrCH] Stands folder not found; no chess
tables`.

## One table

```
<any name>                        tagged ChessTable
  Board
    Plate                         a square BasePart, or FullPlate, or a Model of tiles
    White/
      King, Queen                 BaseParts, named by piece
      Rooks/Rook, Rook
      Knights/Knight, Knight
      Bishops/Bishop, Bishop
      Pawn x8
    Black/
      the same
  Chair1                          contains a Seat
  Chair2                          contains a Seat
  Camera/                         optional mount parts
```

| Instance | Required | Notes |
|---|---|---|
| `Board.Plate` or `Board.FullPlate` | Yes | Sets the geometry. A `Model` uses its bounding box |
| `Board.White`, `Board.Black` | Yes | The pieces, placed in the standard start position |
| `Chair1`, `Chair2` | Yes | Each needs a `Seat` at any depth |
| `Camera` | No | Mount parts for the seated camera |

## Piece naming

A `BasePart` anywhere under `White` or `Black` is a piece if its name is one of:

```
King  Queen  Rook  Knight  Bishop  Pawn
```

Nesting is free. `White/Rooks/Rook` and `White/Rook` both work, which is why the shipped
models group them. Anything with another name is scenery and is ignored.

Colour comes from which folder the part is under. A piece is tagged with its colour on
startup, so a piece that later moves out of its folder still knows what it is.

## Plate size decides the squares

```
one square = Plate.Size.X / 8
```

The plate must be square. If it is a `Model`, the bounding box is used, and any child part
no larger than one and a half squares is treated as a tile and used for highlighting.

That is how square highlights work without you naming eight squares by hand: the tiles are
matched to squares by where they sit.

## Automatic orientation

There is no rotation setting. On startup the system:

1. measures each piece's position on the plate and turns it into a row and column
2. tries the eight ways a board can be oriented
3. keeps the one under which every piece matches the standard starting position

<Callout type="danger" title="Pieces must start in the standard position or the table will not load">

Calibration is an exact match against the normal chess start. If a pawn is on the wrong
square, a knight and a bishop are swapped, or a piece is missing, no orientation matches
and the table fails to set up with `could not calibrate board orientation against the
standard start`.

That is deliberate. A board that loaded with a guessed orientation would play a game where
moves land on mirrored squares, which is much harder to diagnose than a refusal at
startup.

</Callout>

Set the pieces up properly once and the check never fires again.

## Which chair is white

Also derived, not configured. The chair nearer the white king plays white.

Build a table with white's pieces at one end and the chair at that end is white. If the
white king cannot be found, `Chair1` is white and a warning is logged.

## Captured pieces

Captured pieces are moved to the edge of the board in neat rows rather than deleted, then
returned to their starting positions when the board resets.

The starting position of every piece is recorded on startup, so a reset is exact.

## Promotion pieces

Promoting to a queen, rook, bishop or knight clones the corresponding piece from the same
colour's folder, into a `Promoted` folder the system creates in the table model. It is
cleared on reset.

Because clones come from your own models, a promoted queen looks like your queen. If the
named piece is not found the first `BasePart` in the container is used, so an unusually
structured model still gets something.
