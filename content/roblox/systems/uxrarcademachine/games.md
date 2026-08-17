---
title: The games
order: 3
description: What each of the eight games is, and the knobs that change how it plays.
icon: joystick
---

Every game is configured under `Settings.Games`. Four fields are common to all of them;
the rest are specific.

## The common fields

| Field | What it does |
|---|---|
| `Title` | The player-facing name. All copy comes from this |
| `Controls` | The on-screen control layout: `dpad4`, `dpadrotate`, `horizontal` or `none` |
| `Accent` | The game's colour, used through the interface |
| `MaxScore` | The highest score the server will accept. See [Scores](scores.md) |

`Controls = "none"` means no directional pad is drawn; the game is played by tapping the
canvas, which is what `SweetMatch`, `MineGrid`, `PingPong` and `FlapDash` do.

<Callout type="warning" title="Titles are the only player-facing names, keep them original">

No trademarked game name appears anywhere in this product, in copy, ids, file names or
documentation. `Title` drives every string a player sees.

If you rename a game, keep it original. A cabinet labelled with somebody else's trademark
is a takedown risk that lives in your place, not in the system.

</Callout>

## Snake

A grid. Eat food, grow, and die if you hit yourself.

| Field | Default | Effect |
|---|---|---|
| `GridW`, `GridH` | `15`, `15` | Playfield size in cells |
| `StartInterval` | `0.18` | Seconds per step at the start |
| `MinInterval` | `0.07` | The fastest it ever gets |
| `SpeedRamp` | `0.006` | How much each food speeds it up |
| `FoodPoints` | `10` | Score per food |

## Ping Pong

A paddle against an opponent that returns faster each rally.

| Field | Default | Effect |
|---|---|---|
| `PaddleW`, `PaddleH` | `0.22`, `0.025` | Paddle size, as a fraction of the canvas |
| `BallSize` | `0.03` | Ball size |
| `BallSpeed`, `MaxBallSpeed` | `0.62`, `1.5` | Starting and top speed |
| `SpeedUp` | `1.03` | Multiplier applied per hit |
| `AiSpeed` | `0.6` | How fast the opponent tracks the ball |
| `HitPoints`, `ScorePoints` | `1`, `5` | Score for returning, and for a point won |

`AiSpeed` is the difficulty dial. Below the ball's speed the opponent cannot always reach
it, which is what makes the game winnable.

## Star Blitz

Rows of attackers step down the screen. Three lives.

| Field | Default | Effect |
|---|---|---|
| `Rows`, `Cols` | `4`, `7` | The formation |
| `Lives` | `3` | |
| `StepStart`, `StepMin` | `0.55`, `0.09` | Seconds between formation steps, first and fastest |
| `StepX`, `StepDown` | `0.03`, `0.05` | How far it moves sideways and down |
| `FireCooldown`, `MaxBullets` | `0.35`, `2` | The player's rate of fire |
| `BombChance`, `BombSpeed` | `0.6`, `0.55` | Incoming fire |
| `Points` | `{ A = 30, B = 20, C = 10 }` | Score by row |
| `SaucerPoints` | `100` | The bonus target |
| `SaucerMinDelay`, `SaucerMaxDelay` | `8`, `16` | Seconds between bonus passes |

## Block Stack

Falling shapes, cleared lines, rising speed.

| Field | Default | Effect |
|---|---|---|
| `GridW`, `GridH` | `10`, `20` | The well |
| `DropStart`, `DropStep` | `0.8`, `0.06` | Seconds per drop, and how much each level removes |
| `SoftDrop` | `0.055` | Seconds per drop while held down |
| `LockDelay`, `MaxLockResets` | `0.5`, `15` | Time to slide a piece after it lands, and the reset cap |
| `DasDelay`, `DasRepeat` | `0.16`, `0.045` | Held-direction delay and repeat rate |

`DasDelay` and `DasRepeat` are what make movement feel right to anyone who plays this genre.
Change them only if you know what you are changing.

## Sweet Match

Swap adjacent pieces to line up three, against a clock.

| Field | Default | Effect |
|---|---|---|
| `GridW`, `GridH` | `8`, `8` | The board |
| `Colors` | `6` | Distinct pieces. Fewer is easier |
| `Duration` | `60` | Seconds per run |
| `ClearPoints` | `10` | Score per piece cleared |

## Mine Grid

Reveal cells without hitting a mine.

| Field | Default | Effect |
|---|---|---|
| `GridW`, `GridH` | `9`, `9` | The board |
| `Mines` | `10` | Mines in the first board |
| `MinesPerClear` | `2` | Extra mines added each time the board is cleared |
| `RevealPoints`, `ClearBonus` | `5`, `200` | Score per cell and per completed board |
| `ReadyGate` | `false` | No GET READY overlay, because nothing moves on its own |

It is an endless mode rather than a single board: clearing one deals another with more
mines.

## Ghost Maze

Collect pellets, avoid pursuers, power-ups turn it around.

| Field | Default | Effect |
|---|---|---|
| `StepTime` | `0.16` | Seconds per movement step |
| `Lives` | `3` | |
| `PelletPoints`, `PowerPoints` | `10`, `50` | Score per pellet and per power-up |
| `GhostPoints` | `200` | Score for catching a pursuer while powered |
| `PowerTime` | `6` | Seconds a power-up lasts |

## Flap Dash

One input. Fly through gaps.

| Field | Default | Effect |
|---|---|---|
| `Gravity`, `Flap` | `2.4`, `-0.72` | The feel of the whole game |
| `PipeSpeed`, `PipeSpacing` | `0.44`, `0.6` | How fast obstacles arrive |
| `PipeGap`, `PipeWidth` | `0.3`, `0.16` | The gap to fly through |
| `BirdX`, `BirdSize` | `0.3`, `0.085` | Where the player sits and how big they are |

`PipeGap` is the difficulty. `0.3` is forgiving; `0.22` is not.

## A note on units

Fractional values are fractions of the canvas, not studs or pixels, which is what lets one
set of numbers work on a phone and a monitor. Values in seconds are seconds. Grid values
are cells.
