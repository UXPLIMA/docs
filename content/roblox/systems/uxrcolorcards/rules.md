---
title: The deck and rules
order: 3
description: The 108 cards, what each does, scoring, and the last-card call.
icon: scroll-text
---

## The deck

108 cards, built at runtime.

| Card | Count | Per colour |
|---|---|---|
| 0 | 4 | One each |
| 1 to 9 | 72 | Two each |
| Skip | 8 | Two each |
| Reverse | 8 | Two each |
| Draw two | 8 | Two each |
| Wild | 4 | Colourless |
| Wild draw four | 4 | Colourless |

Four colours: red, yellow, green and blue. That is the standard composition, and the reason
a 0 is rarer than a 5.

## Playing a card

A card is playable when it matches the top card by **colour** or by **number or symbol**. A
wild is always playable and sets the colour.

If you have nothing playable you draw. What happens then depends on
[House rules](house-rules.md).

## The action cards

| Card | Effect |
|---|---|
| Skip | The next player loses their turn |
| Reverse | Direction of play flips |
| Draw two | The next player draws two and loses their turn |
| Wild | Choose the colour |
| Wild draw four | Choose the colour, next player draws four and loses their turn |

In a two-player game a reverse acts as a skip, which is standard and worth knowing when a
table empties down to two.

## Points

```lua
Number cards   their face value
Action cards   20
Wilds          50
```

Points are scored by the **winner** from the cards left in everybody else's hands. That is
why a hand full of wilds is a liability: it is the strongest hand to play and the most
expensive to be caught with.

`Points` is one of the leaderboard stats, so a player who wins big hands ranks above one
who wins narrow ones.

## Starting hands

```lua
StartingHand = 7,
Seats = 4,
MinPlayers = 2,
```

Seven cards each, up to four players, minimum two to start.

Raising `StartingHand` makes games longer and reduces the chance of an early finish.
Lowering it below five makes the last-card call happen almost immediately.

## The last card

With one card left a player must call it. The window is short:

```lua
Timing = {
    LastWindowSeconds = 3,
},
```

Three seconds from playing your second-to-last card. Miss it and you take the penalty:

```lua
ResultMessages.LastPenalty = "Missed Last Card! +2",
```

Two cards. This is what makes the endgame tense, and it is switchable: see
`HouseRules.LastCallout` in [House rules](house-rules.md).

## The turn timer

```lua
Timing = {
    MoveSeconds = 25,
    ClockTickRate = 0.25,
},
```

Twenty-five seconds per turn. The clock ticks four times a second so the countdown looks
smooth rather than jumping.

## Winning

The first player to empty their hand wins the game, scores the points from every other
hand, and the table resets after `Timing.ResetDelay` seconds.

A player who leaves mid-game is removed from the hand and the game continues if enough
players remain. Below `MinPlayers`, the game ends.

## The rules are testable

`Shared/CC/Rules.luau`, `Engine.luau`, `Deck.luau` and `Cards.luau` use no Roblox API and
ship with specs:

```lua
require(game.ReplicatedStorage.uxrColorCardsSystem.CC._spec.Run)()
```

If you change a house rule's behaviour, change it there and run those.
