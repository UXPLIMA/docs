---
title: Building a lane
order: 2
description: The world folder, the named parts a lane model needs, and the anchors that override guesswork.
icon: box
---

Lanes are found in a world folder you build. This system expects a **specific model
layout**: parts are found by name, and lanes come in pairs.

## The world folder

```
Workspace/
  uxrBowlingWorkspace/
    Lanes/
      <a lane pair model>
      <another lane pair model>
    Leaderboards/
```

Every `Model` directly under `Lanes` is examined twice, once as lane 1 and once as lane 2.
One model is a **pair** of lanes, which is how real bowling alleys are built and how the
shipped model is laid out.

## The named parts

Inside a lane pair model, `i` is the lane index, 1 or 2:

| Instance | Required | What it is |
|---|---|---|
| `BowlingPins<i>` | Yes | A container holding `Pin1` through `Pin10` |
| `Pin1` … `Pin10` | Yes | The ten pins. Their resting positions become the rack |
| `Pinsetter<i>` | For the animation | The arm that lifts and sets |
| `Sweep<i>` | For the animation | The bar that clears fallen pins |
| `BallReturn<i>` | For the animation | Where the ball comes back to |
| `Lane<i>Monitor` | No | The screen showing the scorecard |
| `KeyPad<i>` | No | Fallback host for the rack prompt |
| `PitPart<i>` | No | The pit trigger behind the pins |
| `Lane_<i>_Bumpers` | For bumpers | The gutter guards |
| `Lane1&2Gutters` | No | The gutter trigger, shared by the pair |
| `Lane1&2Camera` | No | The lane camera, shared by the pair |
| `FoulDetector` with `Lane<i>FoulLine` | Recommended | Marks the foul line |

A lane with no `BowlingPins<i>` container, or with fewer than ten correctly named pins,
fails to register with a clear message. Everything else degrades: a missing monitor means
no scorecard on the wall, missing bumpers mean the bumper toggle does nothing.

<Callout type="warning" title="Pin resting positions are the rack">

Where the ten pins sit when the game starts **is** the rack the pinsetter resets to. There
is no configured triangle.

Set them out correctly in Studio before the first run. If a pin is a stud out of place, it
is out of place for every frame of every game.

</Callout>

## The Anchors folder

Everything positional can be given explicitly rather than derived:

```
<lane pair model>/
  Anchors/
    ThrowSpot1        where the bowler stands
    ThrowSpot2
    PinCam1           the camera looking at the pins
    ReplayCam1        the replay camera
    RackPrompt1       where the rack prompt appears
    WaitSpots1/
      Spot1 … Spot4   where waiting players stand
```

Anchors are preferred whenever present. Without them the system derives what it can from
geometry, using the foul line and the pin centroid to work out which way the lane runs, and
says so in Output.

Derived positions are usable. Anchored ones are correct. On a lane you built yourself,
place the anchors: it takes a few minutes and removes an entire class of "the camera is
looking at a wall" problem.

<Callout type="danger" title="Without a foul line part, a hard-coded fallback position is used">

If `FoulDetector/Lane<i>FoulLine` is missing, the system logs a warning and falls back to a
**fixed world position** that matches the model it was built against.

On your own lane that position is meaningless, and everything derived from it, the throw
spot and the lane direction, will be wrong. Place the foul line part, or place a
`ThrowSpot` anchor, or both.

</Callout>

## Wait spots

`Anchors/WaitSpots<i>/Spot1` to `Spot4` are where players stand while it is not their turn.
One per player up to `Lane.MaxPlayers`.

Without them, waiting players are left where they are, which works but looks untidy on a
busy lane.

## The rack prompt

The prompt that starts a game is placed at `Anchors/RackPrompt<i>` if present. Otherwise it
falls back to `KeyPad<i>/Main`, then to the keypad itself, then to any part in
`BallReturn<i>`.

That chain exists so a lane always gets a prompt somewhere sensible, but the anchor is the
one that puts it where you meant.

## Machinery travel distances

```lua
Machine = {
    SetterY = Vector3.new(0, 2.583, 0),
    SweepY = Vector3.new(0, 3.87, 0),
    SweepX = Vector3.new(12, 0, 0),
},
```

These are how far each part moves, and they match the shipped lane model. If your
pinsetter is a different size, these are the numbers to change; the animation does not
measure your parts.

See [Settings](settings.md) for the timing fields alongside them.

## Several lanes

Add more lane pair models under `Lanes`. Each registers as two lanes with independent
games, players and state.
