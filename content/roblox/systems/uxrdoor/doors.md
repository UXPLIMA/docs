---
title: Tagging a door
order: 2
description: The tags, the DoorId attribute, the panels, and the optional parts.
icon: box
---

A door is a `Model` in the world, tagged and given an id. Nothing is placed for you.

## The two required things

| | |
|---|---|
| Tag the model | `uxrDoor` |
| Set an attribute | `DoorId`, matching a key in `Config/Doors.luau` |

A tagged model with no `DoorId` warns and is skipped. A `DoorId` with no config entry warns
and is skipped. Both messages name the model, so they are quick to fix.

## The tags

| Tag | Applied to | Purpose |
|---|---|---|
| `uxrDoor` | The model | Marks it as a door |
| `uxrDoorPanel` | A `BasePart` | A moving panel |
| `uxrKeycardLight` | A `BasePart` | Flashes green or red on access |
| `uxrHighlight` | A `BasePart` | Highlighted when the player looks at the door |
| `uxrDoorTouch` | A `BasePart` | Opens the door on contact, for touch doors |

All five names are configurable under `Settings.Tags`.

## Panels

Parts tagged `uxrDoorPanel` are the parts that move. Their resting position is recorded as
the closed state, so the door returns exactly where you built it.

```
Door                       tagged uxrDoor, DoorId = "2"
  LeftPanel                tagged uxrDoorPanel, PanelIndex = 1
  RightPanel               tagged uxrDoorPanel, PanelIndex = 2
  HandlePart
  Light                    tagged uxrKeycardLight
```

For a double door, set a **`PanelIndex` attribute** on each panel: `1` and `2`. Panels are
sorted by it, and the motion config's `panels` list is applied in that order.

Without `PanelIndex`, panels are numbered in discovery order, which is not something you
should rely on.

<Callout type="tip" title="An untagged door falls back to its PrimaryPart">

A door model with no tagged panels uses its `PrimaryPart` as the single panel. That is
enough for a simple swing door and saves a tag.

For anything with two panels, or where the primary part is not the door leaf, tag
explicitly.

</Callout>

## HandlePart

```lua
HandlePartName = "HandlePart",
```

A part found by **name**, searched recursively through the model. Three things use it:

| Use | Detail |
|---|---|
| Sounds | Per-door `Sound` instances live inside it, and generated sounds are parented to it |
| The beam | A `Beam` named `uxrDoorBeam` inside it links the door to the player |
| Range | The interact range is measured from it |

Put same-named `Sound` instances inside it to give a door its own audio:

| Sound name | Plays |
|---|---|
| `DoorOpen` | The door opening |
| `DoorClose` | The door closing |
| `AccessGranted` | Access allowed |
| `AccessDenied` | Access refused |
| `Extra1` | The extra sound, for doors with `extraSound = true` |

A `Sound` found by name is played in place. If there is none, the id from
`Config/Sounds.luau` is used instead, as a temporary `Sound` cleaned up after five seconds.

<Callout type="warning" title="Every shipped sound id is an empty string">

```lua
return {
    open = "",
    close = "",
    granted = "",
    denied = "",
    extra = "",
}
```

Doors are silent out of the box. Fill these in for game-wide defaults, or place `Sound`
instances in each `HandlePart` for per-door audio.

</Callout>

A door with no `HandlePart` still works. It falls back to its first panel for sound and for
the range check, and has no beam.

## Keycard lights

Parts tagged `uxrKeycardLight` have their colour set on access events:

```lua
LightColors = {
    grant = Color3.fromRGB(0, 255, 0),
    deny = Color3.fromRGB(255, 0, 0),
    lockdown = Color3.fromRGB(255, 0, 0),
    reset = Color3.fromRGB(255, 255, 255),
},
LightFlashTime = 1,
```

Green on a grant, red on a deny, red while locked down, then back to white after
`LightFlashTime` seconds.

Build them as neon parts. The colour is set on the part, so a `SurfaceLight` or
`PointLight` child does not change with it unless you drive it yourself.

## The prompt

Every door that is not a touch door needs a `ProximityPrompt` named
**`uxrDoorProximityPrompt`**, placed in a part that is a **direct child of the door model**:

```
Door                       tagged uxrDoor, DoorId = "1"
  Panel                    tagged uxrDoorPanel
  Reader                   a part
    uxrDoorProximityPrompt
```

The model is resolved as the prompt's parent's parent, so a prompt nested one level deeper
is not recognised.

The prompt's own `HoldDuration` is what the player holds for. Its `ActionText` is unused for
doors: the label comes from the door's open state. See [Prompts and effects](prompts.md).

## Touch doors

A door with `touch = true` in its config opens when a part tagged `uxrDoorTouch` is
touched, with no prompt.

The debounce is the door's `autoCloseSeconds` plus one, so a player standing in the trigger
does not re-open it every frame.

Touch doors still run the access check. A touch door with a restricted access preset opens
only for the people allowed through it, which is a good pattern for an automatic staff-only
door.

## Adding doors at runtime

Discovery is by tag with a live listener, so a door tagged after the server starts is bound
immediately. A door untagged or removed is unbound.

That makes doors work in a placed building, a loaded map or a player-built structure.

## Several doors, one id

Ten doors carrying `DoorId = "1"` are ten doors behaving identically. That is the intended
use: the id is a **door type**, not an instance.

Give a door its own id only when it needs its own behaviour.
