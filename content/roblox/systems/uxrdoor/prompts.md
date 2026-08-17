---
title: Prompts and effects
order: 6
description: The four prompt styles, sizing, the highlight, the beam, and mobile.
icon: pointer
---

The default Roblox prompt is replaced by a `BillboardGui` of your choosing. Four styles
ship, and holding it fills a progress bar.

## The four types

```lua
Type1 = { template = "uxrDoorCustomPrompt1", fill = "vertical", spin = true },
Type2 = { template = "uxrDoorCustomPrompt2", fill = "vertical" },
Type3 = { template = "uxrDoorCustomPrompt3", fill = "horizontal" },
Type4 = { template = "uxrDoorCustomPrompt4", fill = "vertical" },
```

| Type | Bar | Text | Key |
|---|---|---|---|
| `Type1` | `MainFrame.EffectFrame`, fills upward | `PromptName` | `MainFrame.KeyText` or `KeyImage` |
| `Type2` | `MainFrame.Loadbar.Bar`, fills upward | `PromptName` | `MainFrame.KeyLabel` |
| `Type3` | `MainFrame.Bar`, fills rightward | None | `MainFrame.KeyText`, as `Press <b>E</b> to Interact` |
| `Type4` | `MainFrame.ButtonFrame.Bar`, fills upward | `MainFrame.TextFrame.KeyText` | `MainFrame.ButtonFrame` |

`Type1` also rotates its `BorderFrame` five degrees and back when the hold completes, which
is the `spin` flag.

The four `BillboardGui` templates live under `uxrDoorCustomPrompts` in the shipped
`uxrDoorGui`. Restyle them freely, and keep the instance names above: each is looked up
individually and a missing one is skipped rather than erroring.

## Choosing a type

```lua
DefaultUIType = "Type1",
```

<Callout type="warning" title="The prompt style is global, not per door">

The config comments describe a per-door `uiType`. The client does not read one: it resolves
the style from `DefaultUIType` for every door in the game.

Changing `DefaultUIType` changes all of them. If you need a different style for one door,
that is a change to the client's type resolution, not a config field.

</Callout>

## The key display

The label adapts to what the player is using:

| Input | Shown |
|---|---|
| Keyboard | The key name, from the prompt's `KeyboardKeyCode` |
| Gamepad | The button image, from `GamepadKeyCode` |
| Touch | A tap icon |

The prompt instance in the world owns the keybind, so change `KeyboardKeyCode` there rather
than in config.

## The hold

The hold duration comes from the `ProximityPrompt` instance's own `HoldDuration`, not from
config. Set it in Studio, per prompt.

Releasing early tweens the bar back to zero over 0.2 seconds rather than snapping it, so a
half-press reads as a half-press.

A completed hold is guarded by 0.2 seconds, so the door cannot be double-triggered by one
press.

<Callout type="info" title="Settings.HoldDuration and ClickCooldown are not used">

Both exist in `Config/Settings.luau` and neither reaches the interaction. `HoldDuration` is
sent to the client at bootstrap and then ignored in favour of the prompt's own value, and
`ClickCooldown` is superseded by the fixed 0.2 second completion guard.

Set the hold on the prompt instances. Leave these two alone.

</Callout>

## Sizing

```lua
PromptScale = 1,
```

Billboards are sized as a fraction of screen height rather than a fixed pixel size, so they
look the same on a phone and on a 4K monitor:

```
factor = clamp(viewportHeight / 1080, 0.8, 1.25) * PromptScale
```

The clamp keeps small screens from getting unusably tiny prompts and large screens from
getting comically large ones. `PromptScale` multiplies the result: `1.2` for bigger prompts,
`0.85` for smaller.

Resizing the window recalculates it live.

## The label

A door prompt reads `OPEN DOOR` when the door is closed and `CLOSE DOOR` when it is open,
updating live as the door's state changes for anybody, not only for you.

The terminal prompt uses its own `ActionText`, falling back to `ACCESS`.

<Callout type="warning" title="openText and closeText are configured but not read">

`Config/Doors.luau` documents `openText` and `closeText` per door, with defaults, and the
type definitions include them. The client hard-codes `OPEN DOOR` and `CLOSE DOOR`.

Setting them changes nothing today. To get per-door labels you would change the client's
label logic to read the door's config.

</Callout>

## The highlight

Looking at a door adds a white `Highlight` drawn on top of geometry, fading in over 0.3
seconds and out again when you look away.

```lua
Highlight = { fillTransparency = 0.8, outlineTransparency = 0.3, color = Color3.fromRGB(255, 255, 255) },
```

Those values match what the client uses, but the client does not read them: the fill,
outline and colour are fixed at 0.8, 0.3 and white. Editing this table has no effect.

The `uxrHighlight` tag is defined in `Settings.Tags` and is likewise unused: the highlight
is applied to the whole door model.

## The beam

A `Beam` named **`uxrDoorBeam`** placed inside a door's `HandlePart` links the door to the
player while the prompt is showing. The client attaches its far end to your
`HumanoidRootPart` and detaches it when the prompt hides.

Doors with no such beam simply have none. The per-door `beam` field and
`Settings.Beam.enabled` are not read: the beam exists exactly when you built one.

Give the `Beam` its own `Attachment0` inside the handle. The system supplies only
`Attachment1`.

## Mobile

On a touch device an on-screen button is created while a prompt is showing, and removed when
it hides. Pressing it starts the hold; releasing it cancels; holding it long enough
completes.

The touch path uses a timer rather than the prompt's own trigger, so the fill bar and the
action stay in step on a phone.

## Refusal reasons

Requests that fail return a reason:

| Reason | Meaning |
|---|---|
| `RateLimited` | Faster than 0.4 seconds since the last request |
| `NoDoor` | Not a bound door |
| `TooFar` | Further than `InteractRange`, 18 studs |
| `Busy` | The door is mid-animation |
| `Locked` | The door's zone is locked down |
| `Denied` | The access check failed |

The shipped client does not surface these as text. The lights and sounds are the player's
feedback. The reasons are there for you to build a notification on if your game wants one.
