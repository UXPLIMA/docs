---
title: Physics and aiming
order: 4
description: The simulation parameters, what each one changes, and how the aim line stays honest.
icon: activity
---

Shots are simulated, not thrown into Roblox physics. The server steps a purpose-built
model at a fixed timestep until every ball has stopped, then replays the recorded path to
the clients.

## The parameters

```lua
Config.params = {
    dt = 1 / 240,
    friction = 1.8,
    stopThreshold = 0.03,
    restitution = 0.70,
    spinFactor = 3.0,
    maxTime = 12,
    sampleEvery = 4,
},
```

| Field | Default | What it changes |
|---|---|---|
| `friction` | `1.8` | How quickly balls slow. **Lower rolls further** and feels heavier |
| `restitution` | `0.70` | Cushion bounciness, 0 to 1. Lower settles faster |
| `spinFactor` | `3.0` | How much english curves the cue ball |
| `dt` | `1/240` | The simulation timestep |
| `stopThreshold` | `0.03` | Speed below which a ball is considered stopped |
| `maxTime` | `12` | Simulated seconds before a shot is force-ended |
| `sampleEvery` | `4` | How often the path is sampled for replay |

The three worth touching are `friction`, `restitution` and `spinFactor`. They are the feel
of the game.

The other four are the numerics. `dt` at 1/240 is what makes contacts resolve cleanly;
raising it to 1/60 makes fast balls pass through each other. `sampleEvery` trades replay
smoothness against how much data crosses the network.

<Callout type="warning" title="maxTime is a safety net, not a game rule">

A shot that has not settled after twelve simulated seconds is ended anyway. In normal play
nothing comes close.

If you lower `friction` a long way, shots take longer to settle, and a very low value can
push a hard break past the cap. If balls occasionally stop dead for no reason, that is the
first number to check.

</Callout>

## Table dimensions

```lua
Config.tableConfig = {
    pocketRadius = 0.45,
    ballRadius = 0.22,
},
```

`ballRadius` is the radius the simulation uses, in studs. It does not resize your ball
models: it is the collision size. Keep it close to half the visual diameter or contacts
will look wrong.

`pocketRadius` is the fallback mouth size for tables without designer-placed pocket parts.
Bigger is easier. See [Building a table](table.md).

## Power

```lua
Config.maxPower = 28
Config.minPower = 2
```

Studs per second at a full and an empty power bar. The bar is linear between them.

`minPower` above zero means a tapped shot still moves the ball, which is kinder than a
dead cue at the bottom of the bar.

## The aim line

The client draws the projected path, and it is computed from the same model the server
uses to resolve the shot. That is the point of simulating rather than approximating: what
the aim line shows is what happens.

Spin bends the cue ball's path, and the projection accounts for it, so `spinFactor`
changes both the shot and the line at the same time.

The client's projection is a preview. The server runs the authoritative simulation when
the shot is taken, so a modified client can draw whatever it likes and still get the same
result on the table.

## Sound

```lua
Config.sounds = {
    single = { "rbxassetid://…", … },
    hole = { "rbxassetid://…", … },
    volume = 0.45,
    minImpact = 0,
    fullImpact = 13,
    minVolScale = 0.6,
    clusterWindow = 0.06,
},
```

`single` is the click pool for ball contacts and the cue strike; one is picked at random so
a rally does not sound like a metronome. `hole` is the pocket drop.

Volume scales with impact speed:

| Field | Default | Effect |
|---|---|---|
| `minImpact` | `0` | Below this speed a click is silent. `0` means every touch is heard |
| `fullImpact` | `13` | At this speed the click plays at full `volume` |
| `minVolScale` | `0.6` | The quietest a click can be, so soft taps stay audible |
| `clusterWindow` | `0.06` | Contacts within this many seconds merge into one click |

`clusterWindow` is what stops a hard break firing a dozen sounds at once. It is a small
detail that makes a large difference to how the break sounds.

## Determinism

The same shot from the same table state always produces the same result. That makes the
game fair, makes the aim line trustworthy, and makes the spec suite meaningful: the physics
and rules modules are tested directly rather than by playing.

Run the specs from Studio with the test project connected:

```lua
require(game.ReplicatedStorage.uxrBilliardsSystem.Billiards._spec.Run)()
```
