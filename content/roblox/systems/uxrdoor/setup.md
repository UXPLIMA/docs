---
title: Setup
order: 1
description: Install, replace the owner id, and get one door opening.
icon: play
---

Install with the standard four steps in
[Installing a system](../platform/installation.md): drag `uxrDoorSystem` into
**ServerScriptService** and press Play once.

Output should read `DoorService bound 0 door(s)`. That is correct: you have not tagged
anything yet.

## 1. Replace the owner id

`Shared/Config/Permissions.luau`:

```lua
Owner = { playerIds = { 3057647029 } },
```

That is somebody else's user id, and almost every shipped door uses this preset. Replace it
with your own before anything else, or none of the example doors will open for you.

<Callout type="danger" title="Config/Zones.luau has placeholder ids too">

```lua
local groups = { [111111] = { 0, 1, 2 }, [111112] = { 0, 1, 2, 3 } }
local playerIds = { 3057647029, 111111, 1111112 }
```

Group ids `111111` and `111112` and the trailing user ids are placeholders carried over
from the original build. Replace them with your own group and staff ids, or delete the
factors you do not use.

</Callout>

## 2. Build one door

A `Model` containing:

| Instance | Purpose |
|---|---|
| A part, tagged `uxrDoorPanel` | The door leaf |
| A part named `HandlePart` | Where sounds play from |
| A part with a `ProximityPrompt` named `uxrDoorProximityPrompt` | The interaction |

The prompt's part must be a direct child of the model.

## 3. Tag it and name it

Tag the model `uxrDoor`, then add a **string attribute** `DoorId` with the value `1`.

Door `1` is the shipped Police swing door:

```lua
["1"] = { access = "Police", motion = { type = "swing", angle = 110, seconds = 2 } },
```

## 4. Press Play

Output should now read `DoorService bound 1 door(s)`.

Walk up to the door. The prompt appears, the door highlights, and holding the key gives you
a red light and a denial, because you are not on the Police team.

Set your team to Police, or change the door to `access = "Owner"` now that the owner id is
yours. Hold again: green light, and it swings.

It closes itself after ten seconds, which is the shipped default.

<Callout type="tip" title="Nothing opened and nothing was logged">

Three things to check, in order:

1. The tag is on the **Model**, not on a part.
2. The `DoorId` attribute exists and matches a key in `Config/Doors.luau`. Both failures
   warn in Output with the model's full name.
3. The prompt is named exactly `uxrDoorProximityPrompt` and its part is a direct child of
   the model.

</Callout>

## 5. Try a double door

Give a second model two panels with `PanelIndex` attributes `1` and `2`, and
`DoorId = "2"`:

```lua
["2"] = {
    access = "Owner",
    motion = {
        type = "double",
        seconds = 2,
        panels = {
            { type = "slide", move = Vector3.new(0, 0, 3.65) },
            { type = "slide", move = Vector3.new(0, 0, -3.65) },
        },
    },
},
```

Two panels sliding apart. The `panels` list is applied in `PanelIndex` order, so if they
slide the wrong ways, swap the two entries or swap the indices.

## 6. Place the terminal

A part with a `ProximityPrompt` named **`uxrPCProximityPrompt`**. No tag, no attribute, no
config entry.

Hold it and the terminal opens, listing the zones you have access to. See
[Zones and the terminal](zones.md).

## 7. Give your doors sound

The shipped sound ids are all empty strings. Fill them in `Config/Sounds.luau`, or place
`Sound` instances named `DoorOpen`, `DoorClose`, `AccessGranted`, `AccessDenied` and
`Extra1` inside a door's `HandlePart`.

## 8. Turn Debug off

`Settings.Debug` ships as `true`. Set it to `false` before release.

## 9. Publish

Nothing here needs the live game. Group and gamepass checks behave the same in Studio.
