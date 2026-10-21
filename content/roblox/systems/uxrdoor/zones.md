---
title: Zones and the terminal
order: 5
description: Grouping doors, the three buttons, lockdown, and placing a terminal.
icon: monitor
---

A zone is a set of DoorIds operated together from a terminal screen.

```lua
["1"] = {
    title = "Zone 1",
    doorIds = { "0", "1", "2", "3", "4" },
    access = { teams = { "Police", "Solider" }, groups = groups, playerIds = playerIds },
    buttons = { openAll = false, closeAll = true, lockdown = true },
    order = 0,
},
```

| Field | Meaning |
|---|---|
| `title` | The heading on the terminal card |
| `doorIds` | Which DoorIds this zone operates |
| `access` | Who may operate it, same shape as [Access](access.md) |
| `buttons` | Which of the three actions appear |
| `order` | Sort order in the terminal list |

## Zones operate door types

`doorIds` lists **DoorIds**, not individual doors. A zone naming `"1"` operates every door in
the world carrying `DoorId = "1"`.

The terminal shows a live count of how many doors that actually resolves to, which is the
quickest way to notice a zone that names an id nobody used.

## The three buttons

| Button | Effect |
|---|---|
| `openAll` | Opens every closed, unlocked, idle door in the zone |
| `closeAll` | Closes every open, idle door in the zone |
| `lockdown` | Toggles the zone between `NORMAL` and `LOCKDOWN` |

A button set to `false` does not appear, and the action is refused server side even if
somebody asks for it directly.

The terminal positions whichever buttons are enabled: one button fills the card's width, two
split it left and right.

<Callout type="warning" title="Three enabled buttons are not laid out">

There is no three-button layout. A zone with `openAll`, `closeAll` and `lockdown` all `true`
shows all three at whatever size and position they have in the template, which is not what
you want unless you have restyled the template for it.

Enable at most two buttons per zone, or edit the template so three overlapping buttons sit
sensibly.

</Callout>

## Lockdown

Lockdown is a toggle, not a momentary action. Pressing it once locks the zone; pressing it
again releases it.

While a zone is locked down:

| | |
|---|---|
| Every door in it | Is closed, waiting for anything mid-motion to finish |
| Requests at those doors | Are refused with a denied sound and a red light |
| The keycard lights | Stay red rather than resetting to white |
| Auto close | Does not fire, because the doors are already closed |

Releasing lockdown resets the lights to white and lets the doors work again. It does not
reopen anything.

The zone's status is broadcast to every client, so a terminal open on somebody else's
screen updates as it happens.

<Callout type="info" title="Lockdown state does not survive a restart">

Zone status lives in server memory and starts at `NORMAL`. A server restart clears every
lockdown.

That is usually what you want. If your game needs lockdown to persist, it needs a datastore
write of its own.

</Callout>

## Placing a terminal

A part with a `ProximityPrompt` named **`uxrPCProximityPrompt`**.

There is no tag, no attribute and no config entry. Any prompt with that name opens the
terminal, so a game can have one terminal or twenty, all showing the same zones.

Guard the terminal itself by putting it in a room behind a restricted door. The terminal is
not access-checked on open: it shows only the zones the player passes the access check for,
and a player with no zones sees an empty list.

## What the terminal needs

The terminal screen is built from the shipped `uxrDoorGui`. Its structure is a
loading frame, a main screen and a scrolling list of zone cards cloned from a template:

```
MainFrame
  LoadingScreenFrame
    LoadFrame.BarFrame
  MainScreenFrame
  DoorScreenFrame
    ScrollingFrame
      UIGridLayout
        Template
          Frame
            NavFrame.NavbarLabel      the zone title
            StatusText                NORMAL or LOCKDOWN
            DoorsText                 how many doors resolved
            OpenAllButton
            CloseAllButton
            LockdownButton
    ProgramBar.CloseButton
  CloseButton
```

Restyle any of it. Keep the names: they are looked up individually, and a missing one is
skipped rather than erroring, which makes a renamed part show up as a silently missing
label.

Cards animate in one at a time with a short stagger, so a long zone list fills the screen
progressively.

## Refusal reasons

| Reason | Meaning |
|---|---|
| `NoZone` | No zone with that id |
| `Denied` | The player fails the zone's access check |
| `Disabled` | That button is `false` in the zone config |
| `BadAction` | Not one of the three actions |
| `RateLimited` | Faster than `RateLimit.zone`, 0.6 seconds |

All five are enforced on the server, so the terminal's visible buttons are a convenience
rather than the security boundary.

## Designing zones

A workable pattern for a roleplay map:

| Zone | Buttons | Why |
|---|---|---|
| Cells | `openAll`, `closeAll` | Routine, used constantly |
| Perimeter | `closeAll`, `lockdown` | Never opened remotely |
| Armoury | `lockdown` only | One button, one purpose |

Give each zone the smallest set of buttons that does its job. A zone with an `openAll` that
nobody should press is a zone somebody presses.
