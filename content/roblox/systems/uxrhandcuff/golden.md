---
title: The golden handcuff
order: 6
description: A second tool that carries a player floating in front of you.
icon: sparkles
---

A separate tool that picks a player up and carries them floating ahead of the officer,
instead of the normal grab that walks them along the ground.

It is the premium version: give it to staff, sell it, or use it for a specific role.

```lua
return {
    ToolName = "Golden Handcuff",
    CarryOffset = Vector3.new(0, 0, 6),
    CarryRange = 30,
    LerpAlpha = 0.5,
    RequirePermission = true,
}
```

## Using it

Equip the tool and **click a player**. Click again, or unequip, to put them down.

That is a different interaction from the rest of the system: the normal actions target the
nearest valid player automatically, while this one uses your mouse.

| Field | Meaning |
|---|---|
| `ToolName` | The exact `Tool` name that enables it |
| `CarryOffset` | Where the carried player floats, in studs. Only the Z component is used |
| `CarryRange` | The furthest you can be to **start** a carry |
| `LerpAlpha` | How tightly they follow, 0 to 1 |
| `RequirePermission` | Whether the team rules apply |

## Range is checked once

`CarryRange` of 30 studs gates the pickup only. Once carrying, the player is repositioned in
front of you every frame regardless of distance, so they cannot be left behind.

That is much longer than the 8-stud `ActivationDistance` of the normal actions, which is
part of what makes this the premium tool.

## Following

```lua
LerpAlpha = 0.5,
```

Each frame the carried player is moved a fraction of the way toward a point `CarryOffset.Z`
studs ahead of the officer, facing the same way.

| Value | Feel |
|---|---|
| `1` | Snaps exactly into place. Rigid, and jittery on a bad connection |
| `0.5` | The default. Smooth, with a slight lag on turns |
| `0.2` | Floaty, trailing well behind on fast movement |
| `0` | Does not follow at all |

Their velocity is zeroed each frame, so they do not accumulate momentum and fly off when
released.

## Permission

```lua
RequirePermission = true,
```

| Value | Who may carry |
|---|---|
| `true` | Officers with `GrabPermission` over that target, plus admins |
| `false` | Anybody holding the tool, on anybody |

`true` is the sensible default. `false` is for games where the golden handcuff is a toy
rather than a police tool.

<Callout type="info" title="Carrying does not cuff">

The carry is independent of the three restraint states. A carried player is not marked as
cuffed, their tools are not confiscated, and you cannot open the arrest form on them.

To arrest somebody you carried somewhere, put them down and cuff them normally.

</Callout>

## What ends a carry

| Event | |
|---|---|
| The officer clicks again | Released |
| The officer unequips the tool | Released |
| Either player dies | Released |
| Either player leaves | Released |
| The carried player's character goes away | Released |

Release restores their walk speed, jump height and network ownership.

Two officers cannot carry the same player: a second attempt returns `BadTarget`.

## Rate limit

Toggling is guarded by half a second on the server and 0.4 seconds on the client, so a
double click does not pick somebody up and immediately drop them.

## Setting it up

1. Make a `Tool` named exactly `Golden Handcuff`.
2. Give it to whoever should have it: `StarterPack`, a gamepass script, or a shop.
3. Leave `RequirePermission = true` unless you want it usable on anybody.

The name is the only thing that matters. Model the tool however you like.
