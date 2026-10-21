---
title: Adding a game
order: 4
description: The game contract, the runtime object, and the ready gate.
icon: code
---

A new game is **one module and one settings entry**. The framework, the server and the
cabinets do not change.

## The contract

Create `Client/uxrArcadeMachineSystemGui/CoreScript/Modules/Games/<GameId>.luau`:

```lua
local Game = {}
Game.__index = Game

function Game.new(runtime)
    return setmetatable({ rt = runtime }, Game)
end

function Game:start()
    -- reset state and begin a run
end

function Game:stop()
    -- tear down; null out GUI references so start() rebuilds them
end

return Game
```

Then register it:

```lua
Games = {
    MyGame = {
        Title = "My Game",
        Controls = "dpad4",
        Accent = Color3.fromRGB(120, 220, 90),
        MaxScore = 1000000,
        -- your own knobs
    },
},
```

Set `GameId = "MyGame"` on a cabinet and it is playable.

## The runtime object

Everything a game is given arrives in `runtime`.

| Member | What it is |
|---|---|
| `Canvas` | A `Frame` to draw in, already aspect-ratioed and letterboxed |
| `Input` | The unified input stream |
| `Clock` | Loop hooks |
| `Theme`, `Motion`, `Format` | The shared palette, tweens and number formatting |
| `Settings` | Your entry from `Settings.Games` |
| `GameId` | Your id |
| `Paused` | Read it every tick |
| `Started` | The ready gate, see below |
| `SetScore(n)` | Update the HUD |
| `GameOver()` | End the run |
| `Ready()` | Re-arm the ready overlay mid-run |

Draw inside `Canvas` and nowhere else. The host clears it before every run, and its aspect
comes from your `GridW` and `GridH`, so a phone and a monitor get the same playfield.

## Input

```lua
self.rt.Input:onPress("Left", function() … end)
self.rt.Input:onRelease("Action", function() … end)
local dir = self.rt.Input:direction()
```

Actions are `Up`, `Down`, `Left`, `Right`, `Action`, `Secondary` and `Rotate`. They are
unified across keyboard, the on-screen controls and swipes, so a game never asks which
device it is on.

Which on-screen layout appears comes from your `Controls` field:

| Value | Layout |
|---|---|
| `dpad4` | Four directions |
| `dpadrotate` | Four directions plus a rotate button |
| `horizontal` | Left and right, plus an action button |
| `none` | Nothing drawn. The canvas is tapped directly |

New layouts go in `Controls.luau`. A game asking for a layout that does not exist gets
nothing drawn.

## The loop

```lua
self.rt.Clock:onHeartbeat(function(dt)
    if self.rt.Paused then return end
    if not self.rt.Started then return end
    -- step the game
end)
```

`onHeartbeat` is variable-step; `onRender` exists for drawing. Both are disconnected by
the host when the run stops, so a game never has to remember to clean up its connections.

Check `Paused` every tick. The host pauses on the pause button and when the game-over
screen is up.

## The ready gate

At the start of every run the host shows a GET READY overlay and holds `Started` at
`false` until the first input. Gate your loop on it:

```lua
if not self.rt.Started then return end
```

Without that check a real-time game runs while the overlay is up, and the player loses a
life to a screen they have not seen yet.

Two escapes:

- a game with no motion of its own opts out with `ReadyGate = false` in its settings entry,
  which leaves `Started` at `true`. `MineGrid` does this
- a game that wants the overlay again mid-run, after a lost life or between levels, calls
  `runtime:Ready()`

## Scoring

```lua
self.rt:SetScore(self.score)
…
self.rt:GameOver()
```

`SetScore` only updates the display. `GameOver` ends the run: the host submits the score,
shows the final screen and handles play-again.

Do not submit scores yourself. The server checks the submission against your `MaxScore`,
and a game that talks to the remote directly bypasses the framework's cooldown.

## stop must null its references

```lua
function Game:stop()
    self.board = nil
    self.pieces = nil
end
```

The host clears the canvas between runs, so any `Frame` a game is holding is destroyed. If
`start` does not rebuild from nothing, play-again draws into instances that no longer
exist.

The rule: `start` must be able to run against a fresh, empty canvas every time.

## Guarding delayed work

A death animation scheduled with `task.delay` can fire after the player has already started
a new run. The shipped games guard this with a per-run counter captured at the start and
compared when the delayed work runs.

Do the same for anything that outlives a frame.

## What you never touch

Adding a game does not change `GameHost`, `Input`, `Controls`, `HUD`, `GameOver`,
`Leaderboard`, any server service, or the cabinet contract. If your change needs one of
those, it is a framework change and worth thinking about separately.
