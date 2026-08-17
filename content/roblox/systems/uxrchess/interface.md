---
title: The interface
order: 6
description: The seat panel, the HUD, the promotion picker, and the attributes that wire buttons up.
icon: layout-panel-top
---

`Client/uxrChessSystemGui` is a `ScreenGui`. Everything it contains lives under one frame
called `ChessUI`.

## The instance contract

```
uxrChessSystemGui
  ChessUI
    SeatPanel
      TimeRow                   TextButtons, each with a TcKey attribute
      EloRow                    TextButtons, each with an Elo attribute
      Start                     TextButton
      BotButton
      Waiting
    HUD
      Turn
      WhiteClock
      BlackClock
      Resign
      Draw
    DrawPrompt
      Accept
      Decline
    Promotion
      Panel
        Choices                 one button per piece
    Result                      the end-of-game banner
    Backdrop                    optional dimmer
  CoreScript                    the code. Do not rename or move it
```

If `ChessUI` is missing, Output says so and the interface does nothing while the rest of
the system keeps working.

## Buttons are wired by attribute, not by text

A time-control button is matched to a control by a **`TcKey` attribute** holding a key
from `Settings.TimeControls`:

```
TimeRow/
  Button1    TcKey = "untimed"
  Button2    TcKey = "blitz5"
  Button3    TcKey = "rapid10"
```

A strength button is matched by an **`Elo` attribute** holding a number from
`Settings.EloPresets`.

This is what lets you label a button "Quick game" and still have it mean `blitz5`. A
button with no attribute, or one naming something not in the settings, is ignored.

Adding a time control or a preset to the settings means adding a button with the matching
attribute.

## The HUD

| Instance | Holds |
|---|---|
| `Turn` | Whose move it is |
| `WhiteClock`, `BlackClock` | The two clocks. Hidden on an untimed game |
| `Resign` | Ends the game as a loss. Present only when `AllowResign` is on |
| `Draw` | Offers a draw. Present only when `AllowDraws` is on |

`DrawPrompt` is what the opponent sees when a draw is offered: `Accept` and `Decline`.

`ConfirmResign` makes the resign button ask before it acts, which is worth leaving on: a
misclick that loses a thirty-minute classical game is a support ticket.

## The promotion picker

`Promotion.Panel.Choices` holds one button per piece a pawn can become. It appears when a
pawn reaches the last rank and `Settings.PromotionPicker` is on.

With the setting off, promotion is automatic and always a queen, and the frame is never
shown. That is a reasonable simplification for a casual game; under-promotion matters in
perhaps one game in a thousand.

## The board is in the world

Selecting and moving happens on the board model, not in the interface. The client turns a
click into a square using the same calibrated geometry the server uses, and the server
rejects any move that is not legal for that player at that moment.

So a re-skin cannot break the rules, and a modified client cannot make an illegal move.

## Highlights are on the board

Selection, last move and check are shown by colouring the plate's tiles, not by drawing
over the screen. They need the plate to be a `Model` of per-square parts; a plate that is
one flat part still plays correctly but cannot be highlighted.

## Camera lock

With `CameraLock` on and a `Camera` folder in the table model, a seated player's camera
moves to the mount. Without the folder, the setting does nothing and the player keeps their
normal camera.

Put one mount part per chair, invisible, looking at the board from behind that side.

## Re-skinning it

Edit the copy under `ServerScriptService/uxrChessSystem/Client/`. The bootstrap clones it
into `StarterGui` at startup and only when no `ScreenGui` of that name is already there,
so edits to the `StarterGui` copy last until the next Play.

Frames you add are kept, because the `ScreenGui` ignores unknown instances. Frames the code
looks for by name are not optional.
