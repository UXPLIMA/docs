---
title: In car controls and the HUD
order: 11
description: The lock, horn, eject and radio widget, the driving HUD, and every key.
icon: keyboard
---

## The widget

```lua
Controls = {
    Enabled = true,
    PanicSoundId = "",
    RadioPlaylist = { "rbxassetid://142376088" },
    RadioVolume = 0.5,
    DespawnKey = Enum.KeyCode.X,
},
```

A small panel that appears only while the player is sitting in a `VehicleSeat` in their own
active vehicle.

| Button | Does |
|---|---|
| Lock | Ejects anybody else who sits down, and keeps ejecting them |
| Panic | Plays `PanicSoundId` from the car |
| Eject | Empties every seat in the car, including the driver's |
| Radio | Starts the playlist |
| Next | Skips to the next track |
| Stop | Stops the radio |
| Despawn | Destroys the car |

### Lock

Locking does not stop somebody getting in. It runs a check roughly three times a second and
throws out anybody in any seat who is not the owner.

<Callout type="info" title="The lock is a permanent bouncer, not a door">

There is a visible moment where a stranger sits down before being ejected. That is the
design: it costs nothing to check, and there is no way for a passenger to slip past it.

It also means passengers you actually want are impossible while locked. There is no
allow list.

</Callout>

The lock is cleared when the car changes or the player leaves.

### The radio

The playlist is a list of asset ids. The sound is created on the car's primary part, so
everybody nearby hears it, and it loops until stopped.

```lua
RadioPlaylist = { "rbxassetid://142376088" },
```

<Callout type="warning" title="Replace the shipped track">

One track ships, and it is a placeholder. A single looping song in every car in your game
gets old within a session.

Use audio you own or audio Roblox lets you use. Anything else can be moderated away, which
leaves silent cars and errors in Output.

</Callout>

### The panic sound

`PanicSoundId` ships empty, so the button shows a toast and makes no noise. Point it at a
horn asset to make it work. The sound is cleaned up after six seconds.

## The driving HUD

```lua
DriveHud = {
    Enabled = true,
    EarnPopup = true,
    ShowSpeed = false,
    ShowDistance = false,
    Unit = "metric",
    PopupDuration = 1.2,
    PopupY = 0.25,
    MinPopupAmount = 1,
},
```

Its own ScreenGui, built in code rather than from your design, so there is nothing to lay
out.

| Element | Shown when |
|---|---|
| The earning popup | Each time the player earns while driving |
| Speed and distance | With `ShowSpeed` or `ShowDistance` on |
| The test drive countdown | During a test drive |

`MinPopupAmount` suppresses the popup for small amounts, which stops a stream of `+$1`
labels on a low reward rate.

`Unit` decides km/h and km against mph and mi, and the same choice drives the radar boards.
Set it once, at the start.

## Every key

| Key | Opens |
|---|---|
| G | The general dealership |
| H | The auction panel |
| K | The admin panel, for admins |
| X | Despawns the active vehicle |
| E | Any nearby prompt: dealerships, gas stations, chargers, trunks |
| Escape | Closes the top modal |

All four of the first are configurable: `Dealerships[].OpenKey`, `Auction.OpenKey`,
`Admin.OpenKey` and `Controls.DespawnKey`.

<Callout type="warning" title="Check these against your other systems">

G, H, K and X are common keys. If another system in your game already uses one, whichever
listener runs first wins and the other one looks broken.

Avoid F9, which is the Roblox developer console.

</Callout>

Keys are ignored while the player is typing in a text box.

## Focus mode

```lua
FocusMode = {
    Enabled = true,
    OtherScreenGuis = true,
    KeepVisible = { "uxrNotifySystemLiteGui" },
},
```

While the dealership is open, every other on-screen interface is hidden: the fuel gauge,
the vehicle widget, the open button, and with `OtherScreenGuis` on, other ScreenGuis
entirely, including other systems' interfaces.

Each hidden interface's previous state is remembered and restored on close, so anything
that was already hidden stays hidden. Nothing is force-opened.

`KeepVisible` is the exception list, by ScreenGui name. The notification interface is in it
by default, so toasts still reach the player while they are shopping.

`HideToolsInDealership` does the same job for the player's tools and hotbar.
