---
title: Rule sets
order: 3
description: The four games, the six presets, and what double in and double out do.
icon: scroll-text
---

Four rule sets, each a self-contained module, exposed to players through six presets.

## The presets

```lua
Config.Matches = {
    Practice       = { ruleSet = "X01", startScore = 501, doubleIn = false, doubleOut = false, legsToWin = 1 },
    X01_501        = { ruleSet = "X01", startScore = 501, doubleIn = false, doubleOut = true,  legsToWin = 3 },
    X01_301        = { ruleSet = "X01", startScore = 301, doubleIn = false, doubleOut = true,  legsToWin = 3 },
    AroundTheClock = { ruleSet = "AroundTheClock", legsToWin = 1 },
    Cricket        = { ruleSet = "Cricket",        legsToWin = 1 },
    Shanghai       = { ruleSet = "Shanghai",       legsToWin = 1 },
},
```

| Field | What it does |
|---|---|
| `ruleSet` | Which module runs the game |
| `startScore` | The starting total, for X01 |
| `doubleIn` | Scoring does not begin until a double is hit |
| `doubleOut` | The winning dart must be a double |
| `legsToWin` | Legs needed to take the match |

Add a preset by adding an entry. `X01_701 = { ruleSet = "X01", startScore = 701, doubleOut = true, legsToWin = 5 }`
needs nothing else.

## X01

Count down from the starting score to exactly zero.

Each dart subtracts what it scored. Going below zero, or to one when `doubleOut` is on, is a
**bust**: the visit is void and the score returns to where it was at the start of that
visit.

| Option | Effect |
|---|---|
| `doubleIn` | Nothing counts until the first double is hit |
| `doubleOut` | The dart that reaches zero must be a double or the bull |

`doubleOut` is what makes the last few points the hard part, and it is on for the two real
presets and off for practice. That is the right split: a new player should be able to
finish a leg.

## Around the Clock

Hit 1, then 2, then 3, all the way to 20, then the bull.

Any part of the number counts, so a treble 7 advances you from 7 exactly as much as a
single does. Miss and you throw again at the same target. The leg is won when the bull
falls.

It is the best practice game in the set, and the shortest to explain to somebody who has
never played darts.

## Cricket

Only seven numbers matter: 15, 16, 17, 18, 19, 20 and the bull.

Each needs three marks to close:

| Hit | Marks |
|---|---|
| A single | 1 |
| A double | 2 |
| A treble | 3 |
| Outer bull | 1 |
| Inner bull | 2 |

Once you have closed a number and your opponent has not, further hits on it score points.
The winner is whoever closes everything with the points lead.

Cricket rewards accuracy in a narrow band of the board rather than raw scoring, which makes
it the most different game in the set from X01.

## Shanghai

Seven rounds. In round one only 1 counts, in round two only 2, and so on to round seven.

Scoring is the number times its ring multiplier, so a treble in round five is worth 15. A
dart anywhere else scores nothing.

There is a traditional instant win, hitting a single, double and treble of the round's
number in the same visit, and the classic short format is the seven rounds themselves.

Shanghai is quick, which makes it the right choice for a busy board where people are
queuing.

## Legs

```lua
legsToWin = 3,
```

The match goes to the first player to win that many legs. `1` makes a single leg the whole
match, which is what the practice and side games use.

## Practice

`Practice` is 501, no double in, no double out, one leg. It is the mode to point a new
player at, and the only preset where a leg cannot end in a frustrating chain of busts.

## Adding a rule set

A rule set is a module under `Shared/Darts/RuleSets/` exposing `newPlayerState`,
`applyThrow` and `publicState`. Register it in the table at the top of the table service
and add a preset that names it.

The engine handles turns, visits, legs and the match; a rule set only decides what one
dart does.
