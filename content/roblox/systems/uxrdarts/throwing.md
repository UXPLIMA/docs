---
title: Throwing
order: 4
description: The wobble model, why accuracy improves with play, and the board geometry.
icon: activity
---

A throw is an aim point and a power error. Where the dart actually lands is that aim plus a
random offset.

## The wobble

```lua
Config.Wobble = { Base = 0.25, Power = 0.35 }
```

The spread of the offset is

```
spread = Base * (1 - accuracy) + Power * powerError
```

and the dart lands somewhere uniformly inside a circle of that radius, in board units where
the double wire is at 1.0.

Two things follow.

**`Base` is the skill term.** At accuracy 1.0 it contributes nothing; at accuracy 0.5 it
contributes half of `0.25`, which is an eighth of the board radius. That is the difference
between a player who hits what they aim at and one who does not.

**`Power` is the timing term.** A perfectly timed throw has no power error and no
contribution; a badly timed one adds up to `0.35`, which is a third of the board.

Raise `Base` to make skill matter more and untrained players worse. Raise `Power` to make
the timing meter matter more relative to skill.

## Accuracy improves with play

A player's accuracy is not a setting. It is derived from their record:

```
accuracy = clamp(DartsHit / DartsThrown, 0.5, 1)
```

A brand-new player with no record throws at `0.8`. From then on their accuracy is their
own hit rate, floored at `0.5` so a terrible run never makes the game unplayable, and
capped at `1.0`.

<Callout type="note" title="This is a progression system disguised as a physics model">

A player who plays well throws more accurately, which makes them play better. That is a
positive feedback loop, and it is deliberate: it means a returning player is measurably
better than they were, without a level number anywhere.

It also means a player who spends an evening throwing at nothing in practice can lower
their own accuracy. If that bothers you, exclude practice matches from the hit and throw
counters.

</Callout>

## The board

```lua
Config.Radii = {
    InnerBull = 0.037,
    OuterBull = 0.094,
    TrebleIn  = 0.582,
    TrebleOut = 0.629,
    DoubleIn  = 0.953,
    DoubleOut = 1.0,
},
```

Proportional radii from the centre, with the outside of the double ring at 1.0. These are
real dartboard proportions: the treble ring is narrow, the bull is small, and the gap
between them is most of the board.

| Ring | Scores |
|---|---|
| Inner bull | 50 |
| Outer bull | 25 |
| Treble | Three times the sector |
| Double | Twice the sector |
| Single | The sector |
| Outside the double | Nothing |

Widening `TrebleOut` minus `TrebleIn` makes trebles easier and is the single most effective
way to make the game more forgiving. It also makes 180s common, which devalues the
leaderboard stat that counts them.

## Sector order

```lua
Config.Sectors = { 20, 1, 18, 4, 13, 6, 10, 15, 2, 17, 3, 19, 7, 16, 8, 11, 14, 9, 12, 5 }
```

The genuine clockwise order. It is what makes missing 20 expensive and missing 19
survivable, which is the whole strategic texture of a real dartboard.

Do not reorder it to be "fairer". The arrangement is the game.

## What the server decides

The aim comes from the client, and everything after it is server-side: the wobble, the
sector and ring the dart landed in, the score, the bust check and the leg state.

A modified client can aim wherever it likes. It cannot decide where the dart lands or what
it was worth.
