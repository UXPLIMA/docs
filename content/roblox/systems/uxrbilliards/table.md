---
title: Building a table
order: 2
description: The tag, the parts a table needs, the ball assets, and how the play area is measured.
icon: box
---

Tables are found by **tag**, not by folder. Anything in the world tagged `BilliardsTable`
is examined at startup, and a table added later is picked up as well.

## One table

```
<any name>                       tagged BilliardsTable
  Surface                        a BasePart. The play area
  JoinPad                        a BasePart. Hosts the prompt
  Pockets/                       optional, one BasePart per pocket
  Seats/                         optional, one BasePart per standing position
  CameraPart                     optional, the shot camera mount
```

| Instance | Required | Notes |
|---|---|---|
| `Surface` | Yes | Its size and orientation define the whole play area |
| `JoinPad` | Yes | The prompt is attached here |
| `Pockets` | No | Named parts. Without it, pockets are inferred |
| `Seats` | No | Where joined players stand |
| `CameraPart` | No | Where the camera sits for a shot |

A table missing `Surface` or `JoinPad` warns in Output and is skipped. The rest of your
tables still work.

## Surface defines the play area

The cloth is measured from the `Surface` part: its size gives the table's dimensions and
its orientation gives the direction of play. Everything the simulation does is in the local
frame of that part.

Build the surface as the playable cloth only, not the rails and not the frame. If the part
includes the wooden edge, balls will bounce off a cushion that is not where the cushion
looks.

## Pockets

With a `Pockets` folder, each `BasePart` in it becomes a pocket: its position gives the
location and its smaller horizontal dimension gives the mouth radius.

Name them meaningfully. The names are what a called-pocket shot refers to, so `TopLeft`
and `SideRight` read better in a log than `Part` and `Part1`.

Without the folder, `Config.tableConfig.pocketRadius` is used with the standard six
positions.

<Callout type="tip" title="Designer-placed pockets beat the global radius">

`pocketRadius` is one number for every table in the game. Real table models differ: a
decorative table with wide pocket mouths should be easier to sink on than a tournament
one.

Placing pocket parts lets each table carry its own difficulty, and makes the pocket a
player can see match the pocket the simulation uses.

</Callout>

## The balls

Ball models are **not** part of the table. They live in the package:

```
ServerScriptService/uxrBilliardsSystem/Storage/Assets/Balls/
  WhiteBall
  1 … 15
```

One `BasePart` per ball, named by its number, plus `WhiteBall` for the cue ball.

At startup the set is cloned once per table into a runtime folder, anchored and made
non-collidable: they are moved by the simulation, not by Roblox physics. Between shots they
are parked out of sight below the table.

Because every table clones the same set, re-skinning the balls is one edit that applies
everywhere.

## Seats

`Seats` holds one part per standing position. A player who joins is moved to a free one and
their movement is frozen for the duration, so nobody walks off mid-shot or stands in the
camera.

Their walk speed and jump are restored when they leave. Two seats is the usual number;
extra seats are spectator positions.

## The camera

`CameraPart` is where the shot camera sits. Without it, the player keeps their normal
camera, which is playable but much harder to aim from.

Point it down the length of the table from above, high enough to see the whole cloth.

## The prompt

Created for you on `JoinPad`, named `BilliardsPrompt`, reading **Join Pool Game** on a
**Pool Table**, within twelve studs and not requiring line of sight.

You do not add it, and it is not currently configurable from the settings file. To change
the wording, edit the prompt creation in `BilliardsTableService`.

## Several tables

Copy the model, keep the tag. Each table clones its own balls, runs its own session and
keeps its own state. Nothing is shared but the leaderboards and the profiles.
