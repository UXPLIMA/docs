---
title: The interface
order: 13
description: The ScreenGui contract, the template pattern, and what a missing name costs you.
icon: layout-panel-left
---

The interface is `uxrVehicleSystemPackGui`, a designed ScreenGui you edit in Studio.
Restyle it freely; keep the names.

## Top level

Every one of these is a direct child of the ScreenGui.

| Child | Holds |
|---|---|
| `MainFrame` | The dealership |
| `ButtonFrame` | The open button. Any `GuiButton` inside it toggles the dealership |
| `FuelGauge` | The fuel readout |
| `VehicleWidget` | The in-car controls |
| `TrunkFrame` | The trunk |
| `AuctionFrame` | The auction panel |
| `AdminFrame` | The admin panel |
| `CoreScript` | The client code. Do not rename or move it |

Only `MainFrame` is required. Every other panel is looked up with a soft find, so a
ScreenGui without an `AuctionFrame` simply has no auction panel and everything else works.

## The dealership

```
MainFrame
  TopBar
    BalanceLabel          the player's money
    SlotLabel             the garage counter
    CloseButton
  LeftPanel
    ListFrame
      CardTemplate        one per vehicle, cloned
    FilterBar
    SearchBox
    SortButton
    OwnedToggle
  CenterPanel
    OrbitCatcher          the 3D preview surface
    PurchaseButton
    TestDriveButton
  RightPanel
    NameLabel
    CategoryLabel
    DescriptionLabel
    PriceLabel
    StatsFrame
      StatTemplate        one per stat row, cloned
    ColorPalette
      SwatchTemplate      one per palette color, cloned
    PlateButton
    SellButton
    SaleLabel
```

## The template pattern

Three lists are built by cloning a template you design:

| Container | Template |
|---|---|
| `ListFrame` | `CardTemplate` |
| `StatsFrame` | `StatTemplate` |
| `ColorPalette` | `SwatchTemplate` |
| `AuctionsList` | `AuctionRowTemplate` |
| `Detail.OwnedList` | `OwnedRowTemplate` |
| `TrunkList` and `InventoryList` | Their own row templates |

The template itself stays in place, hidden, and clones are made from it. Everything else in
the container is destroyed on each rebuild.

<Callout type="info" title="Design one row, get the whole list">

Style the template and every row follows. Layout comes from a `UIListLayout` or
`UIGridLayout` you add yourself, and the code sets `LayoutOrder` so the order is yours to
sort.

Keep the template's `Visible` property off in Studio. Clones are made visible explicitly.

</Callout>

A missing template logs a warning naming the exact path, and that list stays empty. Watch
Output while restyling and it tells you what you renamed.

## The trunk

```
TrunkFrame
  TrunkList          what is in the trunk
  InventoryList      what is in the backpack
  CapacityLabel
  CapacityBar
    Fill
  SharePanel
  ShareButton
  CloseButton
```

`ShareButton` is hidden unless the viewer is the owner and `Trunk.AllowSharing` is on.

## The vehicle widget

```
VehicleWidget
  NameLabel
  LockBtn
  PanicBtn
  EjectBtn
  RadioBtn
  NextBtn
  StopBtn
  DespawnBtn        optional
```

Every button except `DespawnBtn` is required: the widget indexes them directly, so a
missing one is an error rather than a warning. `DespawnBtn` is looked up softly.

`LockBtn` has its text and background colour set by the code, so style everything else about
it and leave those two alone.

## The fuel gauge

```
FuelGauge
  BarBg
    BarFill
  Label
```

`BarFill` is scaled horizontally, so anchor it to the left of `BarBg` and give it a full
height size. Its colour is set by the code.

## Interfaces built in code

Three things are not in your ScreenGui at all:

| | |
|---|---|
| The driving HUD | Its own ScreenGui, created at runtime |
| The radar banner | Created at runtime |
| Modals and toasts | Built by the modal and notification modules |

They inherit the theme rather than a design, which is why `NotifyStyle` exists in settings.

## Deploying your changes

Edit the copy inside
`ServerScriptService/uxrVehicleSystemPack/Client/uxrVehicleSystemPackGui`. That is the
design-time copy.

<Callout type="warning" title="StarterGui is only seeded once">

The bootstrap copies the ScreenGui into `StarterGui` only if a ScreenGui of that name is
not already there. It never overwrites.

That means a stale copy left in `StarterGui` from an earlier run silently wins over every
change you make to the package copy. If your edits are not showing up, delete the
`StarterGui` copy and press Play again.

</Callout>

## Theme

The client modules share one theme table: a dark panel, a card colour, an accent, and
success, warning and danger colours. The code sets colours from it in a handful of places,
listed above, and leaves everything else to your design.
