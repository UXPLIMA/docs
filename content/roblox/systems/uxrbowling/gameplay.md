---
title: Playing a game
order: 3
description: The lobby, turn order, scoring, bumpers and the replay.
icon: scroll-text
---

## Joining

A player walks up to the lane and uses the rack prompt. The first one to join starts a
countdown:

```lua
Lane = {
    LobbyCountdownSeconds = 10,
    MinPlayers = 1,
    MaxPlayers = 4,
},
```

The game starts when the countdown ends, or immediately when the lane fills to
`MaxPlayers`. `MinPlayers = 1` means a lone player can bowl.

A player arriving at a lane with a game already running is told so and not queued. Lanes
are first come, first served.

## Turns

Players bowl in join order, one frame each, ten frames.

```lua
TurnSeconds = 30,
```

A player who does not throw within `TurnSeconds` has a gutter ball thrown for them and the
turn moves on. That is the right behaviour: the alternative is one AFK player holding four
others hostage for the rest of a game.

## Scoring

Standard ten-pin.

| Frame | Result | Scores |
|---|---|---|
| A strike | Ten pins with the first ball | 10 plus the next two deliveries |
| A spare | Ten pins across two balls | 10 plus the next delivery |
| Open | Fewer than ten | The pins knocked down |

The tenth frame gives an extra delivery on a strike or a spare, so a perfect game is twelve
strikes and 300 points.

The scoring module is pure logic with no Roblox API and ships with a spec, so it is
correct by test rather than by inspection. It is also the place to look if you want a
non-standard variant.

```lua
Game = { Frames = 10, PinCount = 10 },
```

Both are configurable, and both should be left alone unless you deliberately want a
non-standard game. A five-frame game works; the scoring still applies bonuses across frame
boundaries as it should.

## Counting pins

The system waits for everything to stop moving and then measures which pins are still
standing:

```lua
SettleSeconds = 2.5,
PinUprightDot = 0.85,
PinMoveThreshold = 1.5,
```

| Field | Default | Meaning |
|---|---|---|
| `SettleSeconds` | `2.5` | How long to wait after the ball reaches the pins |
| `PinUprightDot` | `0.85` | How upright a pin must still be to count as standing |
| `PinMoveThreshold` | `1.5` | How far it may have moved and still count |

A pin that is leaning past the dot threshold, or has slid more than the move threshold, is
counted as down.

If counts look wrong, `SettleSeconds` is the first thing to check: too short and a pin that
was about to topple is counted as standing.

## Bumpers

```lua
Bumper = { DefaultOn = false },
```

Gutter guards. Off by default and toggled in the lobby before a game starts, which is the
right shape: it is a choice the group makes, not a server-wide policy.

Set `DefaultOn = true` for a game aimed at younger players.

Bumpers need a `Lane_<i>_Bumpers` object in the lane model. Without one the toggle does
nothing.

## The replay

```lua
Replay = { Enabled = true, MaxSeconds = 5, HoldSeconds = 7 },
```

A slow-motion replay of each shot, with its own camera anchor and its own music track.

| Field | Default | Meaning |
|---|---|---|
| `Enabled` | `true` | Play replays at all |
| `MaxSeconds` | `5` | Longest a replay runs |
| `HoldSeconds` | `7` | How long the game waits before the next turn |

<Callout type="warning" title="HoldSeconds must be larger than MaxSeconds">

The hold is what gives the replay room to finish. Set it below `MaxSeconds` and the next
turn begins while the replay is still playing, cutting it off.

The shipped gap of two seconds is a reasonable margin.

</Callout>

Turning replays off shortens every turn by roughly `HoldSeconds`, which is worth doing on a
busy alley where lanes queue.

## Ending

When the last player finishes their tenth frame, the scoreboard holds:

```lua
ResultsDisplaySeconds = 6,
```

Then the lane empties and is free for the next group. `0` clears instantly.

The winner is whoever scored highest. Rewards are paid, and stats are recorded. See
[Stats and leaderboards](stats.md).
