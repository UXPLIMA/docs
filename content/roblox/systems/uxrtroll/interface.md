---
title: The interface
order: 5
description: The panel structure, the instance names, and what a missing one costs you.
icon: layout-panel-left
---

The panel is `uxrTrollSystemGui`, deployed to `StarterGui`. Restyle it freely; keep the
names.

## The structure

```
uxrTrollSystemGui
  MainFrame
    MoveFrame
      NameTextLabel            who you are watching
      BackImageButton          previous target
      NextImageButton          next target
      CloseFrame
        CloseImageButton
    ButtonsFrame
      ListFrame
        Row1Frame
          TrollFrame
            ClickTextButton    opens the troll panel
    OneFrame                   the single-target side
      ButtonsScrollingFrame
        KillFrame
          EventImageButton
            PriceTextLabel
        SlowFrame
        FreezeFrame
        FireFrame
        FlingFrame
        ExplodeFrame
        KickFrame
    AllFrame                   the everyone side
      ButtonsScrollingFrame
        ... the same seven frames
```

## The button contract

Each troll needs a frame named `<Key>Frame` inside the right
`ButtonsScrollingFrame`, holding an `EventImageButton`, holding a `PriceTextLabel`.

| Instance | Purpose |
|---|---|
| `<Key>Frame` | The button's container. Hidden when the troll is unavailable |
| `EventImageButton` | What the player presses |
| `PriceTextLabel` | The live price, or `FREE (n)` |

The seven keys are `Kill`, `Slow`, `Freeze`, `Fire`, `Fling`, `Explode` and `Kick`, matching
the config exactly.

<Callout type="info" title="Missing instances warn rather than break">

Every lookup is checked and logged by name: a missing `AllFrame` warns that side of the
panel is not wired, a missing `KillFrame` warns it is absent from that list, and a frame
without an `EventImageButton` or a `PriceTextLabel` warns too.

The rest of the panel keeps working. Watch Output while restyling and the warnings tell you
exactly what you renamed.

</Callout>

## Spectating

`MoveFrame` is the target browser:

| Instance | Does |
|---|---|
| `NameTextLabel` | Shows the current target's display name |
| `BackImageButton` | Previous target |
| `NextImageButton` | Next target |
| `CloseFrame.CloseImageButton` | Stop watching, camera returns to you |

The camera follows the selected player, and keeps following them through respawns. Closing
the panel puts your camera back on your own character.

When nobody is selected, the label shows your own display name.

## Opening the panel

`ButtonsFrame.ListFrame.Row1Frame.TrollFrame.ClickTextButton` is the entry point. It is a
single row in a list frame, which is how it is meant to sit alongside your game's other
buttons.

## Price labels

Each `PriceTextLabel` shows one of two things:

| State | Text |
|---|---|
| Normal | The live price, formatted by `Price.Format` |
| Credit held | `Price.Free`, with the count |

Both formats are in `TrollSettings.Price`. See [Products and credits](products.md).

Prices are fetched once and then reused, so a label that reads nothing briefly on join is
the fetch still in flight.

## Chat lines

Announcements go to the general text channel with the colour from `Announce.Color`. They
appear in the normal chat window rather than in a custom widget, so they inherit whatever
chat styling your game already has.

## Deploying your changes

Edit the panel inside
`ServerScriptService/uxrTrollSystem/Interface/uxrTrollSystemGui`. That is the design-time
copy.

<Callout type="warning" title="The StarterGui copy is replaced on every server start">

Unlike some of the other systems here, this one destroys any existing same-named ScreenGui
in `StarterGui` and deploys a fresh copy every time.

Changes made directly in `StarterGui` are lost. Always edit the package copy.

</Callout>
