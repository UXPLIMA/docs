---
title: Courts and boards
order: 2
description: The DartsCourt tag, what a court needs, and where the board itself comes from.
icon: box
---

Places to play are found by **tag**. Anything in the world tagged `DartsCourt` is wired up,
including one added while the server is running.

## A court

```
<any name>                    tagged DartsCourt
  Board                       the board model or part
  Avatar                      optional, where the opponent stands
```

A court is where a player walks up and starts a match. The prompt is attached to it, and
the camera framing comes from it.

## The board is drawn, not built

The dartboard a player aims at is drawn by the interface, from
`Config.Sectors` and `Config.Radii`. Your `Board` model is the physical prop: it is what the
room looks like, and it is where thrown darts appear.

This is the opposite of the chess and pool systems in this range, where the world model
**is** the play surface. Here the world model is scenery and the play surface is the
interface, which is what makes a dartboard playable on a phone.

The consequence: you can build any board you like, at any scale, and the game still scores
correctly. A board whose printed numbers do not match the standard order will confuse
players, but it will not affect the scoring.

<Callout type="tip" title="Match your board art to the real sector order">

```
20, 1, 18, 4, 13, 6, 10, 15, 2, 17, 3, 19, 7, 16, 8, 11, 14, 9, 12, 5
```

Clockwise from the top. Players who know darts read the board before they read the
interface, and a mismatch reads as a bug.

</Callout>

## Several courts

Tag as many as you like. Each is an independent place to start a match.

## The world folder

The leaderboards use their own folder, separate from the courts:

```
Workspace/
  uxrDartsWorkspace/
    Leaderboards/
```

Courts are found by tag anywhere; boards are found in that folder. See
[Stats and leaderboards](stats.md).

## Assets

Dart models and other props live in the package at
`ServerScriptService/uxrDartsSystem/Storage/Assets` and are cloned to `ServerStorage` at
startup, with a preview copy in `ReplicatedStorage` so the client can show them.

Replacing the dart model there changes it at every court.
