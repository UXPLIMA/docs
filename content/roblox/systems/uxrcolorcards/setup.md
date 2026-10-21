---
title: Setup
order: 1
description: Install, build a table, and play a hand against the bots.
icon: play
---

Install with the standard four steps in
[Installing a system](../platform/installation.md): drag `uxrColorCardsSystem` into
**ServerScriptService** and press Play once.

## 1. Build the world folder

```
Workspace/
  uxrColorCardsWorkspace/
    Tables/
```

## 2. Put a table in it

A model containing:

| Instance | Purpose |
|---|---|
| `DiscardAnchor` | A flat, face-up `BasePart` where the pile is rendered |
| `Chair1` to `Chair4` | Each containing a `Seat` |

Number the chairs clockwise around the table. Turn order follows the numbers, so a reverse
card then visibly reverses.

## 3. Press Play and sit down

Sit in `Chair1`. The table waits for players, and you can start a game with bots in the
empty seats.

## 4. Play a hand

Play a card, draw when you cannot, and get down to one card. If you do not call it within
three seconds you take two cards back.

That is `LastCallout`, and it is the rule most worth deciding on before launch. See
[House rules](house-rules.md).

## 5. Check the discard pile reads

Cards should lie flat on the anchor, face up, readable from where a seated player is
looking. If they stand on edge, the anchor is not flat.

## 6. Add your sounds

Every sound id ships empty, so the game is silent. Seventeen events are wired up and their
volumes are already balanced; you only need to paste ids.

Start with `CardPlay`, `CardDraw`, `YourTurn` and `LastCall`. Those four carry most of the
feel.

## 7. Decide the house rules

```lua
HouseRules = {
    Stacking = true,
    LastCallout = true,
    DrawUntilPlayable = false,
    SevenZero = false,
    JumpIn = false,
    StrictWildFour = false,
},
```

The shipped set is the game most players expect. Note that `SevenZero` and `JumpIn` are
read by nothing: setting them changes no behaviour.

## 8. Build the leaderboards

```
Workspace/
  uxrColorCardsWorkspace/
    Leaderboards/
      WinsLeaderboard
      PointsLeaderboard
```

One model per entry in `Leaderboard.Stats`, with a
`ListPart.ListGui.ScrollingFrame` holding a `Template` row.

## 9. Publish

Global boards accumulate only in the live game. In Studio they read empty.
