---
title: Cabinets
order: 2
description: The world folder, binding a cabinet to a game, and the prompt.
icon: box
---

The bootstrap does not create cabinets. It looks for a folder you built and attaches
itself to what it finds.

## The world folder

```
Workspace/
  uxrArcadeMachineWorkspace/
    Stands/
      SnakeStand/
        ArcadeModel/
          ScreenPart
      BlockStackStand/
        …
```

Both folder names are settings: `WorkspaceFolder` and `StandsFolder`. The default names
are the ones above.

Without the folder, no cabinets exist and nothing else happens.

## One cabinet

| Instance | Required | Notes |
|---|---|---|
| The stand model | Yes | One per cabinet, any name |
| A `BasePart` inside it | Yes | Hosts the prompt. `ScreenPart` by convention |
| `GameId` attribute | Recommended | Which game this cabinet plays |

Everything else about the model is yours: the cabinet art, the marquee, the screen bezel,
lighting.

## Binding a cabinet to a game

Set a **`GameId` string attribute** on the stand, or on its `ArcadeModel`:

```
GameId = "Snake"
```

The value must be a key in `Settings.Games`. A cabinet whose id is not in the settings is
skipped, and the prompt opens nothing.

If no attribute is set, the stand's **name** is used as a fallback: `SnakeStand` maps to
`Snake`, and a stand named exactly like a game id maps to itself. The attribute is the
supported way; the name map exists so a quickly built test cabinet works.

## The prompt

At startup each cabinet is tagged `ArcadeMachine` and given a `ProximityPrompt` named
`ArcadePrompt`, carrying its `GameId`.

You do not add the prompt yourself. Its behaviour comes from the settings:

```lua
Prompt = {
    ActionVerb = "Play",
    HoldDuration = 0,
    MaxDistance = 12,
    RequiresLineOfSight = false,
},
```

| Field | Default | Effect |
|---|---|---|
| `ActionVerb` | `"Play"` | The verb on the prompt |
| `HoldDuration` | `0` | Seconds to hold. `0` is a tap |
| `MaxDistance` | `12` | How close a player must be |
| `RequiresLineOfSight` | `false` | Whether a wall between them blocks it |

`RequiresLineOfSight = false` is the friendlier default in a busy arcade room where other
players' avatars stand between the player and the cabinet.

## The camera

```lua
Camera = { Lock = false },
```

Off by default: the game is a full-screen interface, so moving the world camera adds
nothing. Turn it on if your cabinets sit in a scene you want framed while playing.

## ScreenPart

`ScreenPart` is the part convention for the cabinet's screen face. Today it hosts the
prompt; it is reserved for an attract-mode `SurfaceGui`. Naming it correctly costs nothing
and keeps your models compatible.

## Adding cabinets at runtime

Cabinets are scanned at startup. A cabinet added to `Stands` later is picked up as well, so
a cabinet spawned by your own code works without a restart.

## Several cabinets, one game

Perfectly fine. Two `Snake` cabinets are two places to play the same game, sharing one
leaderboard and one personal best. Nothing is per cabinet except where it stands.
