---
title: Settings
order: 7
description: The six config files, the global tunables, and what is wired versus what is not.
icon: sliders-horizontal
---

## The six config files

| File | Holds |
|---|---|
| `Config/Doors.luau` | One entry per DoorId: access, motion, timings |
| `Config/Permissions.luau` | Named access presets |
| `Config/Zones.luau` | Zone groups and their terminal buttons |
| `Config/Prompts.luau` | The four prompt UI templates |
| `Config/Sounds.luau` | Default sound ids |
| `Config/Settings.luau` | Everything on this page |

## Settings

| Field | Default | What it does |
|---|---|---|
| `Debug` | `true` | Extra `[uxrDS]` lines in Output |
| `DefaultUIType` | `"Type1"` | The prompt style, for every door |
| `HoldDuration` | `1` | Not used. The prompt instance's own value wins |
| `ClickCooldown` | `0.5` | Not used |
| `PromptScale` | `1` | Multiplier on the screen-relative prompt size |
| `RateLimit.door` | `0.4` | Seconds between accepted door requests per player |
| `RateLimit.zone` | `0.6` | Seconds between accepted zone actions per player |
| `LightColors` | Green, red, red, white | Keycard light colours for grant, deny, lockdown, reset |
| `LightFlashTime` | `1` | Seconds a grant or deny flash holds before resetting |
| `Highlight` | 0.8 / 0.3 / white | Not used. The client hard-codes the same values |
| `Beam.enabled` | `true` | Not used. A beam exists if you built one |
| `Tags.door` | `"uxrDoor"` | The door model tag |
| `Tags.panel` | `"uxrDoorPanel"` | The moving panel tag |
| `Tags.light` | `"uxrKeycardLight"` | The keycard light tag |
| `Tags.highlight` | `"uxrHighlight"` | Not used |
| `Tags.touch` | `"uxrDoorTouch"` | The touch trigger tag |
| `DoorIdAttribute` | `"DoorId"` | The attribute naming a door's config entry |
| `HandlePartName` | `"HandlePart"` | The part sounds, beams and range measure from |
| `InteractRange` | `18` | Studs. The server-side range check |

<Callout type="info" title="Turn Debug off before you publish">

It ships as `true`. Leave it on while you are tagging doors, because the two most common
mistakes, a missing `DoorId` and an id with no config entry, are logged with the model's
full name.

Turn it off for release.

</Callout>

## What is configured but not wired

Five things are documented in config and do not reach the runtime. None of them break
anything; they are simply inert.

| Setting | Reality |
|---|---|
| `Settings.HoldDuration` | The prompt instance's `HoldDuration` is what the player holds |
| `Settings.ClickCooldown` | A fixed 0.2 second completion guard is used instead |
| `Settings.Highlight` | The client hard-codes 0.8, 0.3 and white |
| `Settings.Beam.enabled` | A beam shows when a `uxrDoorBeam` exists in the handle |
| `Tags.highlight` | The highlight adorns the whole model |

And two on the door entries:

| Field | Reality |
|---|---|
| `openText`, `closeText` | Labels are hard-coded `OPEN DOOR` and `CLOSE DOOR` |
| `beam` | Not read. See `Settings.Beam.enabled` above |

The per-door `uiType` described in the config comments is likewise not read. See
[Prompts and effects](prompts.md).

They are listed here so you do not spend an evening changing a value that was never going
to do anything.

## The tag and attribute names

Renaming a tag in `Settings.Tags` renames it everywhere the server looks for it, so a game
that already uses `uxrDoor` for something else can move this system's tag out of the way.

Two names are **not** covered by that, because they are constants in the client rather than
settings:

| Name | Fixed |
|---|---|
| `uxrDoorProximityPrompt` | The door prompt |
| `uxrPCProximityPrompt` | The terminal prompt |
| `uxrDoorBeam` | The beam inside a `HandlePart` |

## Rate limits and range

```lua
RateLimit = { door = 0.4, zone = 0.6 },
InteractRange = 18,
```

These three numbers are the system's security boundary. Every request is checked against
them on the server before anything else happens, so a client that ignores the prompt
entirely still cannot open a door it should not.

Raise `InteractRange` only for doors where the prompt genuinely sits far from the handle.
The check measures to `HandlePart`, so moving the handle is usually the better fix.

## Per-door defaults

`Config/Doors.luau` applies these to every door that does not set them:

```lua
local DEFAULTS = {
    openText = "OPEN DOOR",
    closeText = "CLOSE DOOR",
    beam = true,
    justCheckHand = false,
    autoCloseSeconds = 10,
}
```

Of those, `justCheckHand` and `autoCloseSeconds` are the two that matter. Change the
defaults here rather than repeating the same value on thirty doors.

## Where to start

| You want | Edit |
|---|---|
| Different people through a door | `Config/Permissions.luau`, then the door's `access` |
| A door to move differently | The door's `motion` in `Config/Doors.luau` |
| Doors to stay open longer | `autoCloseSeconds`, per door or in `DEFAULTS` |
| A different prompt look | The `uxrDoorCustomPrompt` billboards, and `DefaultUIType` |
| Sound | `Config/Sounds.luau`, or `Sound` instances in each `HandlePart` |
| A new control panel | `Config/Zones.luau` |
