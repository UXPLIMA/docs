---
title: Color and plates
order: 6
description: How the body color is found and repainted, the five recolor modes, and number plates.
icon: palette
---

## How the body color is found

There is no "body" list to maintain. The system works out which parts are the body by
looking at colors.

```lua
Recolor = {
    Mode = "player",
    FixedIndex = 1,
    PartName = "Color",
    Tolerance = 0.03,
},
```

| Step | What happens |
|---|---|
| 1 | Every part's original color is remembered in a `uxrOrigColor` attribute |
| 2 | The most common color among parts named `PartName` becomes the body color |
| 3 | If no part carries that name, the most common color in the whole model is used |
| 4 | Every part within `Tolerance` of the body color is repainted |

`Tolerance` is per channel, from 0 to 1. The shipped `0.03` is about eight points out of
255, which catches the same paint on doors, panels and mirrors while leaving the tyres and
glass alone.

<Callout type="tip" title="Name the body parts and stop guessing">

Naming the body parts `Color` makes the whole thing deterministic: those parts are always
repainted, and everything that happens to share their original color goes with them.

Set `PartName = ""` to rely entirely on the most common color. That works on tidy models
and repaints the windows on untidy ones.

</Callout>

Because the original color is stored on each part the first time it is painted, repainting
never drifts. The comparison is always against the model's factory colors, not against
whatever it was last painted.

## The five modes

| Mode | The player |
|---|---|
| `"player"` | Picks from the vehicle's `Colors` palette |
| `"picker"` | Picks any color from a hue and saturation picker |
| `"random"` | Gets a random palette entry, assigned once, not editable |
| `"fixed"` | Gets palette slot `FixedIndex`, the same for everybody |
| `"off"` | Nothing is shown and nothing is repainted |

Set `RecolorMode` on a catalog row to override the global mode for one car. The shipped
Starter Hatch does exactly that: the game is in `"player"` mode, and that one car opens the
free picker.

### The palette

```lua
Colors = {
    Color3.fromRGB(196, 60, 66),
    Color3.fromRGB(30, 90, 200),
    Color3.fromRGB(245, 245, 245),
    Color3.fromRGB(20, 20, 24),
    Color3.fromRGB(40, 160, 90),
},
```

Swatches are shown in order. A car with no `Colors` list shows no swatches, so in `"player"`
mode it simply cannot be recoloured; in `"picker"` mode the palette is ignored entirely.

Choices are saved per player per vehicle: the index for a palette pick, a six-digit hex
string for a picker pick. Both are validated on the server, so a client cannot store
something the palette does not contain.

<Callout type="warning" title="Reordering a palette repaints everybody's car">

Palette picks are saved as the slot number. Insert a color at the top of the list and every
saved choice shifts down one.

Append new colors to the end. If you must reorder, accept that existing owners will find
their cars a different color.

</Callout>

Random and fixed choices are assigned the first time the car is spawned or a dealership is
opened, then saved, so the car keeps the same color forever after.

## Number plates

```lua
Plate = {
    Mode = "player",
    RandomFormat = "AA 0000",
    MaxLength = 8,
    Country = "AG",
    InputFrameName = "InputFrame",
    CountryFrameName = "CountryFrame",
},
```

| Mode | The player |
|---|---|
| `"player"` | Types their own plate |
| `"random"` | Gets a generated plate, not editable |
| `"off"` | No plates. The button is hidden and nothing is written |

### The model contract

The plate is written into the model's GUI, not onto a texture. Two named `Frame` instances
anywhere in the vehicle model:

| Frame named | Gets |
|---|---|
| `InputFrameName` | The plate text |
| `CountryFrameName` | The `Country` string |

The first `TextLabel` or `TextBox` inside each frame receives the text. Both frames may
appear more than once, front and rear, and all of them are written.

`Country = ""` leaves the country frame alone, keeping whatever the model shipped with.

### What a player may type

| Rule | |
|---|---|
| Uppercased | Always |
| Letters, digits and single spaces only | Everything else is stripped |
| Trimmed | Leading and trailing spaces removed |
| Truncated | To `MaxLength`, which ships as 8 |

Then the result goes through Roblox text filtering, and the **filtered** text is cleaned
again before it is saved. A plate that comes back hashed out is saved hashed out, which is
the correct outcome for somebody trying to write something they should not.

An empty submission removes the plate.

<Callout type="info" title="Random plates read their format literally">

`RandomFormat` treats `A` as a random letter and `0` as a random digit. Every other
character is a literal.

`"AA 0000"` gives plates like `KP 4821`. `"AAA-000"` loses the hyphen to sanitising and
becomes `ABC000`, so stick to letters, digits and spaces.

</Callout>

## Both together

Paint and plate are applied at spawn time, in that order, before the car is placed and
before anybody sits in it. There is no visible repaint.

The garage preview uses the same two helpers, so the car on the turntable shows the saved
paint and plate and repaints live as the player moves the picker, before anything is sent
to the server.
