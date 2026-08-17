---
title: Settings
order: 6
description: The system-level fields, with defaults.
icon: sliders-horizontal
---

`Shared/Config/Settings.luau`. The per-game entries under `Games` are documented in
[The games](games.md); this page covers the rest.

## System

| Field | Default | What it does |
|---|---|---|
| `Debug` | `false` | Extra `[uxrAM]` lines in Output |
| `Tag` | `"ArcadeMachine"` | The tag applied to each cabinet at runtime |
| `WorkspaceFolder` | `"uxrArcadeMachineWorkspace"` | The world folder |
| `StandsFolder` | `"Stands"` | The folder of cabinets inside it |
| `SubmitCooldown` | `1` | Seconds between score submissions, per player |

The tag is applied by the system, not by you. It is there so your own code can find every
cabinet with one query.

## Prompt

| Field | Default | What it does |
|---|---|---|
| `Prompt.ActionVerb` | `"Play"` | The verb shown |
| `Prompt.HoldDuration` | `0` | Seconds to hold. `0` is a tap |
| `Prompt.MaxDistance` | `12` | Interaction range in studs |
| `Prompt.RequiresLineOfSight` | `false` | Whether something between blocks it |

## Camera

| Field | Default | What it does |
|---|---|---|
| `Camera.Lock` | `false` | Move the world camera while playing |

Off by default. The game is a full-screen interface, so the world camera is not visible
during a run.

## Profile

| Field | Default | What it does |
|---|---|---|
| `Profile.Store` | `"uxrAM_Profile"` | Base name of the save slot |
| `Profile.Version` | `1` | Schema version, appended as `_v1` |

The store used is `Store .. "_v" .. Version`. Bumping `Version` gives everyone a fresh
profile.

## Leaderboard

| Field | Default | What it does |
|---|---|---|
| `Leaderboard.Enabled` | `true` | Publish and read scores |
| `TopN` | `25` | Rows on the panel |
| `RefreshSeconds` | `30` | How often the cached table is re-read |
| `Store.Prefix` | `"uxrAM_LB_"` | Ordered store prefix |
| `Store.Version` | `"v1"` | Bump to start a fresh season |

See [Scores and leaderboards](scores.md).

## Messages

| Key | Default |
|---|---|
| `GameOver` | `"GAME OVER"` |
| `NewBest` | `"NEW BEST!"` |
| `Empty` | `"No scores yet. Be the first!"` |

## Games

One entry per playable game, keyed by game id. Four fields are required on every entry:

```lua
MyGame = {
    Title = "My Game",
    Controls = "dpad4",
    Accent = Color3.fromRGB(120, 220, 90),
    MaxScore = 1000000,
},
```

Everything else in an entry is read by that game and nothing else, which is why the eight
shipped entries look so different from each other.

Removing an entry removes the game. Cabinets bound to it stop opening anything, so remove
the cabinets too.
