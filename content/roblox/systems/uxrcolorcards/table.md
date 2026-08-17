---
title: Building a table
order: 2
description: The world folder, the discard anchor, the chairs, and the turn sign.
icon: box
---

Tables are found in a world folder you build. The bootstrap creates nothing.

## The world folder

```
Workspace/
  uxrColorCardsWorkspace/
    Tables/
      <a table model>
    Leaderboards/
```

Both names are settings: `WorkspaceFolder` and `TablesFolder`.

Anything under `Tables` containing a `DiscardAnchor` is treated as a table, including one
added while the server is running.

## One table

```
<any name>
  DiscardAnchor              a BasePart. The discard pile sits here
  Chair1                     contains a Seat
  Chair2
  Chair3
  Chair4
```

| Instance | Required | Notes |
|---|---|---|
| `DiscardAnchor` | Yes | A `BasePart`. Missing means the table is skipped |
| `Chair<i>` | At least one | Each must contain a `Seat` at any depth |

The number of chairs looked for comes from `Settings.Seats`, which ships at `4`. A table
with two chairs works and seats two.

Missing either piece warns in Output and leaves the rest of your tables alone.

## The discard anchor

The face-up pile is rendered at this part. Its position and orientation decide where cards
appear and which way up they read.

Put it flat in the middle of the table, facing up. A vertical anchor makes the pile stand
on edge, which is not a bug but is rarely what you meant.

## Chairs

Sitting in a chair claims that seat. Standing up leaves the game.

Seats are numbered by chair name, and turn order follows those numbers. `Chair1` to
`Chair4` going clockwise around the table means play goes clockwise, which is worth doing
because a reverse then visibly reverses.

## Bots fill the empty seats

A game can start with fewer humans than seats, with bots taking the rest. See
[Bots](bots.md).

## The turn sign

The system renders a marker showing whose turn it is. It is built at runtime rather than
placed, so there is nothing to add to the model.

## Card art

63 card images ship with the system, listed in `Settings.CardImages` and keyed by card:

```lua
CardImages = {
    ["+2_blue"] = 140138778201962,
    ["block_red"] = 123842221128179,
    ["reverse_green"] = 133910706978036,
    ["color"] = 107921512193043,
},
```

| Key shape | Card |
|---|---|
| `<0-9>_<colour>` | A number card |
| `+2_<colour>` | Draw two |
| `block_<colour>` | Skip |
| `reverse_<colour>` | Reverse |
| `color` | Wild |
| `color_<colour>` | Wild after a colour is chosen |
| `+4_color`, `+4_<colour>` | Wild draw four, before and after |

To re-skin the deck, replace the asset ids. The keys are what the code looks up, so keep
them exactly.

## Colours

```lua
Colors = {
    Red = { 226, 42, 42 },
    Yellow = { 247, 197, 22 },
    Green = { 46, 176, 74 },
    Blue = { 32, 116, 232 },
    Wild = { 22, 22, 28 },
},
```

Red, green and blue from 0 to 255. These tint the interface, the turn sign and the colour
picker. Match them to your card art or the colour choice will not read.

## Several tables

Copy the model. Each runs its own game with its own seats. Nothing is shared but the
leaderboards.
