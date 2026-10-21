---
title: The throw
order: 4
description: The three timed meters, the hook, and the physics values behind the ball and pins.
icon: activity
---

A throw is three timed decisions and then real physics.

## The three meters

Each meter sweeps back and forth and the player taps to lock it in.

| Meter | Setting | Default | Sets |
|---|---|---|---|
| Direction | `DirectionHz` | `0.34` | The aim angle |
| Power | `PowerHz` | `0.46` | The ball's speed |
| Spin | `SpinHz` | `0.41` | How much the ball hooks |

The `Hz` value is how fast the bar sweeps. **Higher is faster and harder**; lower is slower
and easier. The shipped values make direction the most forgiving and power the least, which
is the right ordering: missing the line matters more than missing the speed.

## Ranges

| Field | Default | Meaning |
|---|---|---|
| `MaxAngleDeg` | `45` | Widest aim, left or right |
| `PowerMin` | `28` | Ball speed at the lightest tap |
| `PowerMax` | `60` | Ball speed at full power |
| `SpinMax` | `1.0` | Maximum hook, where `0` is none |
| `StrafeRange` | `2.5` | How far the bowler may step sideways before throwing |

The gap between `PowerMin` and `PowerMax` is how much a soft throw differs from a hard one.
Narrowing it makes power matter less, which makes the game easier and duller in equal
measure.

## The hook

```lua
CurveForce = 40,
CurveDurationSeconds = 1.2,
```

After release the ball is pushed sideways for `CurveDurationSeconds` with `CurveForce`,
scaled by the spin the player set. Bigger values give a sharper, longer hook.

That is what makes an approach from the outside line viable, and it is the setting to raise
if experienced players say the game feels flat.

## Ball and pin physics

These are real Roblox physics values applied to your parts.

| Field | Default | What it changes |
|---|---|---|
| `BallDensity` | `5` | Ball weight. Heavier ploughs through pins |
| `BallFriction` | `0.35` | Grip on the lane. Too high and it stalls |
| `BallElasticity` | `0.05` | Bounce. Keep near zero |
| `PinDensity` | `3` | Pin weight. Heavier topples solidly |
| `PinFriction` | `0.4` | How far pins slide once hit |
| `PinElasticity` | `0.05` | Keep near zero so pins do not bounce upright |

<Callout type="danger" title="Keep the ball and pin densities within about five times each other">

The ball to pin weight ratio is what makes a strike look like a strike. Push it far apart
and the simulation gets unstable: a very heavy ball makes pins explode across the room, a
very light one bounces off the head pin.

`5` against `3` is the tuned pair. Change both together if you change either.

</Callout>

## Where the camera comes from

The throw camera is **not** a setting. It follows the camera mount part placed in the lane
model in Studio.

To reposition it, move the part. That keeps the camera correct per lane, which a global
setting could not do for an alley whose lanes face different directions.

## Ball spawn

```lua
BallSpawnOffset = Vector3.new(0, 0, 0),
```

A nudge applied to where the ball appears. Usually zero; it exists for a lane model whose
ball return sits slightly off the line.

## Tuning order

If the throw does not feel right, change things in this order and test after each:

1. `PowerMin` and `PowerMax`, the range of a throw
2. `CurveForce`, how much a hook actually bends
3. `BallDensity` against `PinDensity`, how the pins react
4. The three `Hz` values, how hard the meters are

Doing it in the other order means retuning the meters against a ball whose behaviour you
then change.
