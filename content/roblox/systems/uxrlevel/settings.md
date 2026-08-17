---
title: Settings
order: 4
description: Saving, Studio behaviour, and the admin amount cap.
icon: sliders-horizontal
---

`Shared/Config/Settings.luau` holds the parts that are not about your ladders.

| Field | Default | What it does |
|---|---|---|
| `Debug` | `true` | Extra `[uxrLS]` lines in Output |
| `StudioMode` | `"auto"` | Whether Studio sessions read and write the `DataStore` |
| `DataStoreName` | `"LevelData_v0"` | The save slot for XP and playtime |
| `AutosaveInterval` | `300` | Seconds between automatic saves of every online player |
| `MaxAdminXPAmount` | `999999999` | The largest amount one admin panel action may apply |

## StudioMode

| Value | Behaviour |
|---|---|
| `"auto"` | Do not save in Studio, save in the live game. The default |
| `true` | Never save. Every session starts from defaults, anywhere |
| `false` | Always use the `DataStore`, including Studio |

`"auto"` is the right answer almost always. It lets you test a ladder without your Studio
sessions writing into the profiles real players will load.

Use `false` only when you are specifically testing that saving works, and remember that
your Studio session is then editing live data.

## DataStoreName

Changing it points the system at an empty store: everyone starts from zero XP and zero
playtime. Keep it stable on a live game.

Versioning it, `LevelData_v0` to `LevelData_v1`, is the deliberate way to wipe every
player's progress, for example after redesigning the ladders so the old numbers no longer
mean anything.

## AutosaveInterval

Every `AutosaveInterval` seconds the server writes back every online player's profile.
Data is also saved when a player leaves and when the server shuts down.

Lower means less lost on an unclean crash and more `DataStore` budget spent. `300` is a
sensible middle for most experiences. Individual API calls can force a save on the spot
with the `save` option, which is what the admin panel does.

## MaxAdminXPAmount

Every amount coming from the admin panel is floored, clamped to zero at the bottom and to
this number at the top. It exists to make a mistyped field a nuisance rather than an
incident.

Lower it to something your staff would plausibly need, for example `100000`. It only
constrains the panel; your own server code calling the API is not limited by it.

## Debug

Leave it on while setting a game up. Turn it off before release: the loop that grants XP
runs for every player on a schedule, and its logging is not free.

Errors and warnings, for example a missing tool or an unauthorised panel attempt, are
printed regardless of this setting.
