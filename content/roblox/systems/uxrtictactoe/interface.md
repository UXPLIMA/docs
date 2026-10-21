---
title: The interface
order: 6
description: The seat panel, the HUD, the result banner, and the instance names they need.
icon: layout-panel-top
---

`Client/uxrTicTacToeSystemGui` is a `ScreenGui`. It shows only while a player is seated at
a table.

## The instance contract

```
uxrTicTacToeSystemGui
  TTTUI
    SeatPanel
      SizeRow                   TextButtons, each with a numeric Size attribute
      DifficultyRow             TextButtons, each with a string Difficulty attribute
      Start                     TextButton
    BotButton
    Waiting
    HUD
      Turn
      Clock
      Resign
    Result                      TextLabel, the result banner
  CoreScript                    the code. Do not rename or move it
```

## The seat panel

Shown to a seated player before a game starts.

`SizeRow` holds one `TextButton` per board size. The button is matched to a size by a
numeric **`Size` attribute** on it, not by its text, so you can label a button "Classic"
and give it `Size = 3`.

`DifficultyRow` works the same way with a string **`Difficulty` attribute** matching a key
in `Settings.Difficulties`. Adding a difficulty to the settings means adding a button with
the right attribute.

Only sizes in `Settings.BoardSizes` do anything. A button carrying `Size = 7` is ignored.

## The HUD

| Instance | Holds |
|---|---|
| `Turn` | Whose move it is, from `Settings.Messages` |
| `Clock` | The per-move countdown. Hidden when `MoveSeconds` is `nil` |
| `Resign` | A button. Resigning ends the game as a loss |

## The result banner

`Result` is a `TextLabel`. It shows one of the `ResultMessages` strings and stays up for
`ResetDelay` seconds, during which the win line is highlighted on the board itself.

## The board is in the world, not on screen

Clicking a cell happens on the board model, not in the interface. The client turns a click
into a cell index using the same geometry the server uses, and the server rejects an
index that is not a legal move for that player at that moment.

That means a re-skin of the interface cannot break the board, and a modified client
cannot place a mark out of turn.

## Mobile

A responsive module scales the interface for small screens. Keep buttons at a size a thumb
can hit, and keep corner radii small: the house style is square-ish, and a large radius on
a small button reads as a different product.

## Re-skinning it

Edit the copy under `ServerScriptService/uxrTicTacToeSystem/Client/`. The bootstrap clones
it into `StarterGui` at startup and only when no `ScreenGui` of that name is already
there, so edits made to the `StarterGui` copy last until the next Play.

Frames you add in Studio are kept, because the `ScreenGui` is marked to ignore unknown
instances. Frames the code looks for by name are not optional; renaming one stops that
part of the interface updating and leaves the rest working.
