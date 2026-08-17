---
title: The interface
order: 7
description: The two frames, the instance names each one needs, and the progress bar.
icon: layout-panel-top
---

`Client/uxrLevelGui` is a `ScreenGui` with two frames: the player's XP display and the
staff panel. You own how both look.

## MainFrame

The XP display. It appears on an XP gain or a level up and hides itself again.

```
uxrLevelGui
  MainFrame
    LevelLabel          "LEVEL 3", or "LEVEL UP!" on a rank change
    NextRank            "Cadet > Officer"
    XPValue             "+75", or "LEVEL 4" on a rank change
    XPAmountLabel       "1240/2000"
    Progressbar
      Bar               resized by scale, 0 to 1 on the X axis
```

| Instance | What the code writes |
|---|---|
| `LevelLabel` | `Text` |
| `NextRank` | `Text` |
| `XPValue` | `Text` |
| `XPAmountLabel` | `Text` |
| `Progressbar.Bar` | `Size`, tweened |

Everything else is yours: the background, the frame's position, the fonts, a stroke, an
icon. The code writes text and one size.

## The progress bar

`Bar` is resized with `UDim2.fromScale(fraction, 1)`, so it must be a child of
`Progressbar` and sized by scale on the X axis for the animation to read correctly. Give
it an anchor point on the left, otherwise it grows from its centre.

On an XP gain the bar is snapped to the old value and then tweened over two seconds to the
new one, which is why a gain that crosses a rank boundary is shown as a fill followed by
the level-up frame rather than a single jump.

## AdminFrame

The staff panel, opened by the admin chat command.

```
uxrLevelGui
  AdminFrame
    PlayerFrame
      SelectedPlayerValue     a StringValue holding the chosen player's name
    TeamFrame
      SelectedValue           a StringValue holding the chosen team
    XPFrame
      TextBox                 digits only, defaults to 1
```

The panel also needs the buttons that submit the three actions, and the lists it fills
with online players and configured teams. Rows are built by cloning your template, and any
`UIListLayout` or `UIPadding` in the container is left alone.

## The two chat commands

| Command | Opens | Configured in |
|---|---|---|
| `!xp`, `!showxp` | `MainFrame`, the player's own XP | `Levels.GuiCommand.Aliases` |
| `!xppanel` | `AdminFrame` | `Admin.AdminCommand` |

Both are exact string matches on the whole chat message. `!xp please` does not match.

## Re-skinning it

Edit the copy under `ServerScriptService/uxrLevelSystem/Client/`, not the one in
`StarterGui`. The bootstrap clones the package copy into `StarterGui` at startup and only
when no `ScreenGui` of that name is already there, so edits to the `StarterGui` copy
survive until the next Play.

The `CoreScript` folder is code. Do not rename it or move it out of the `ScreenGui`.

<Callout type="tip" title="Missing instances warn rather than error">

The context waits ten seconds for `MainFrame` and `AdminFrame` and then carries on. If
you rename a frame, the parts of the interface that use it stop updating while the rest
of the system, XP, ranks, tools and saving, keeps working normally.

That is worth knowing when something looks broken: check Output before assuming the
server side is at fault.

</Callout>
