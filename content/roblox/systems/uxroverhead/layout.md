---
title: The layout
order: 2
description: The row list, the placeholders, and the OverheadUI template contract.
icon: layout-panel-top
---

`Shared/Config/Layout.luau` decides what appears above a head and in what order.

```lua
return {
    { Template = "AFKFrame",       Content = "AFK - {AFKTime}", Order = 1 },
    { Template = "NametagsFrame",  Order = 2 },
    { Template = "TeamLabel",      Content = "{TeamName}", TextColor = "{TeamColor}", Order = 3 },
    { Template = "NameLabel",      Content = "{DisplayName}", TextColor = "{NameColor}", Order = 4 },
    { Template = "RankLabel",      Content = "{RankName}", TextColor = "{RankColor}", Order = 5 },
    { Template = "FrameBar",       Order = 6 },
}
```

| Field | What it does |
|---|---|
| `Template` | The name of a child of the template container. Required |
| `Content` | The text, with placeholders |
| `TextColor` | A colour placeholder, or a hex string like `"#FFD700"` |
| `Order` | Sort order within the nametag |

An entry whose `Template` does not exist is skipped silently. That is how you remove a row:
delete the entry, or delete the template.

## The template contract

The nametag is built from a `BillboardGui` that ships in the package:

```
ServerScriptService/uxrOverheadSystem/Storage/Assets/
  OverheadUI                        a BillboardGui
    UIListLayout                    the template container
      AFKFrame
        AFKLabel
        AFKBLabel
      NametagsFrame
      TeamLabel
      NameLabel
      RankLabel
      FrameBar
```

Templates live **inside the `UIListLayout`**, which is what keeps them from rendering: a
`GuiObject` parented to a layout is not laid out. Clones are parented to the
`BillboardGui`, where the layout arranges them.

That is the same pattern as the reward cards in the daily rewards system, for the same
reason.

<Callout type="warning" title="Move a template out of the UIListLayout and it becomes a visible row">

The templates are invisible because of where they sit, not because of a `Visible` flag.
Reparent one to the `BillboardGui` and it renders permanently for every player.

</Callout>

## Text placeholders

| Placeholder | Resolves to |
|---|---|
| `{DisplayName}` | The player's display name |
| `{Username}` | Their account name |
| `{TeamName}` | Their team, or `TeamUnrankedTitle` |
| `{RankName}` | Their matched rank, or `RankUnrankedTitle` |
| `{Level}` | Their level, from uxrLevel if integrated |
| `{LevelName}` | Their level's name |
| `{AFKTime}` | The AFK timer, as minutes and seconds |

Placeholders can be mixed with text: `Content = "Lv. {Level} {DisplayName}"` works.

## Colour placeholders

| Placeholder | Resolves to |
|---|---|
| `{TeamColor}` | The rank's `TeamColor`, or the Roblox team colour |
| `{RankColor}` | The rank's `RankColor`, or grey |
| `{NameColor}` | The rank's `NameColor`, or white |

Or give a hex string directly:

```lua
{ Template = "NameLabel", Content = "{DisplayName}", TextColor = "#FFD700", Order = 4 },
```

The fallbacks matter: a player who matches no rank still gets a readable nametag, in grey
and white, rather than an invisible one.

## Frames as well as labels

If a template is a `TextLabel`, `Content` sets its text. If it is a `Frame`, the content is
applied to **every** `TextLabel` inside it.

That is how a decorated row works: build a frame with a background, an icon and a label,
and the layout entry fills the label without knowing about the rest.

## AFKFrame is special

The AFK row is cloned like the others and then hidden. The AFK service shows it when the
player goes idle and hides it again when they return.

It needs `AFKLabel` and `AFKBLabel` inside it. See [AFK and health](afk.md).

## Reordering

`Order` is the only thing that decides position, so moving a row is changing a number.
Removing the team row from the top is deleting one entry.
