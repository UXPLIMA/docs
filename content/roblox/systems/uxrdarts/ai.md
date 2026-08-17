---
title: The AI
order: 5
description: The five tiers, what smart checkout means, and the personas.
icon: cpu
---

A player alone at a board plays the AI. It picks a target with the same rule knowledge a
person has, then throws at it with its own wobble.

## The five tiers

```lua
Config.AITiers = {
    Noob   = { accuracy = 0.55, power = 0.45, smartCheckout = false, rating = 600 },
    Easy   = { accuracy = 0.68, power = 0.32, smartCheckout = false, rating = 1000 },
    Medium = { accuracy = 0.80, power = 0.20, smartCheckout = true,  rating = 1400 },
    Hard   = { accuracy = 0.90, power = 0.12, smartCheckout = true,  rating = 1800 },
    Pro    = { accuracy = 0.97, power = 0.05, smartCheckout = true,  rating = 2200 },
},
```

| Field | What it does |
|---|---|
| `accuracy` | How tight its grouping is, on the same scale as a player's |
| `power` | Its timing error, feeding the same wobble term |
| `smartCheckout` | Whether it plays the finish properly |
| `rating` | The ELO it is treated as having |

`Noob` at `0.55` accuracy is roughly a new human player floored at `0.5`, which is the
point: the easiest AI is beatable by somebody who has never played.

`Pro` at `0.97` with almost no power error groups tightly on the treble 20. It is beatable,
because darts is, but not often.

## Smart checkout

The three lower tiers score at the board and finish when they happen to be able to. The
three higher ones plan the finish: with 40 left they throw at double 20, with 32 at double
16, and they set up a double they can hit rather than leaving an odd number.

That single flag is a bigger difference than the accuracy numbers. A `Medium` AI that
finishes properly beats an `Easy` AI that scores better and then cannot get out.

## The personas

```lua
Config.AIPersonas = {
    { name = "Max",  userId = 156 },
    { name = "Ava",  userId = 2 },
    { name = "Leo",  userId = 3 },
    { name = "Nova", userId = 1 },
},
```

Four opponents with names and avatars, so a match is against somebody rather than against
"AI Medium".

The user ids are what the avatar is loaded from. The shipped ones are very low numbers,
which are real, ancient Roblox accounts. Point them at accounts you control, or at least at
ones whose appearance you have looked at.

## Rating

An AI match is rated against the tier's `rating` number, so beating `Pro` moves a player
much more than beating `Noob`, and losing to `Noob` costs.

That is consistent, and it makes the tier choice meaningful rather than cosmetic. It also
means a player can grind a rating by beating a tier slightly below their level over and
over, which is true of every ELO system with a chooseable opponent.

If you would rather AI matches were unrated, the hook context carries `vsBot`; excluding
them is a change in the stats service.

## Adding a tier

Add an entry to `Config.AITiers` and it appears as an option. A tier between `Medium` and
`Hard`:

```lua
Tough = { accuracy = 0.85, power = 0.16, smartCheckout = true, rating = 1600 },
```

Keep `rating` consistent with the accuracy, or the ladder stops meaning anything.
