---
title: Setup
order: 1
description: Install, register a lane, and bowl a frame.
icon: play
---

Install with the standard four steps in
[Installing a system](../platform/installation.md): drag `uxrBowlingSystem` into
**ServerScriptService** and press Play once.

Then give it a lane.

## 1. Build the world folder

```
Workspace/
  uxrBowlingWorkspace/
    Lanes/
```

The folder name comes from `Settings.Leaderboard.WorkspaceFolder`, so it is shared with the
leaderboards.

## 2. Put a lane pair model in it

Every `Model` under `Lanes` provides two lanes, numbered 1 and 2. Its parts are found by
name with the index appended: `BowlingPins1`, `Pinsetter1`, `Sweep1`, and the same for 2.

The full list is in [Building a lane](lane.md). The one that must be right is
`BowlingPins<i>` holding `Pin1` through `Pin10`.

## 3. Place the anchors

```
Anchors/
  ThrowSpot1, ThrowSpot2
  PinCam1, PinCam2
  ReplayCam1, ReplayCam2
  RackPrompt1, RackPrompt2
  WaitSpots1/Spot1 … Spot4
```

Optional but strongly recommended. Without them positions are derived from geometry, and
without a foul line part the derivation falls back to a fixed position that only suits the
model this system was built against.

## 4. Press Play and read Output

Turn `Settings.Debug` on for this step. Lane registration logs exactly what it found and
what it had to guess:

| Output | Means |
|---|---|
| `Anchors folder FOUND` | Positions come from your anchors |
| `Anchors folder MISSING` | Everything is derived from geometry |
| `throwSpot derived from geometry` | No `ThrowSpot` anchor for that lane |
| `FoulLine part not found` | The fallback position is in use. Fix this |
| `10 pins resolved` | The rack is good |

## 5. Bowl a frame

Use the rack prompt. The lobby countdown runs, then you are at the throw spot with the
three meters.

Tap direction, tap power, tap spin, and watch the ball. Then check three things:

- the pins fall like pins, not like paper or like bowling balls
- the count matches what you can see standing
- the pinsetter and sweep animate over the right distances

## 6. Tune the machinery

```lua
Machine = {
    SetterY = Vector3.new(0, 2.583, 0),
    SweepY = Vector3.new(0, 3.87, 0),
    SweepX = Vector3.new(12, 0, 0),
},
```

These are travel distances matched to the shipped lane model. If your pinsetter dives
through the floor or stops halfway, these are the numbers, not a bug.

## 7. Add your sounds

Several sound ids ship real and several ship as `rbxassetid://0`, which means silent:

| Sound | Ships |
|---|---|
| `BallRoll`, `PinHit`, `Strike`, `ReplayMusic` | Real ids |
| `Spare`, `GutterBall`, `Ambience`, `UIClick` | Silent |

Fill in the silent ones before launch. A spare with no sound feels like a bug to a player.

## 8. Publish

Leaderboards accumulate only in the live game. In Studio the global boards read empty.
