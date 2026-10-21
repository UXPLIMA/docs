---
title: House rules
order: 4
description: The six switches, what each changes, and which two are not implemented.
icon: gavel
---

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

Every card game of this kind is played differently in different houses. Four of these
switches change the game.

## Stacking

```lua
Stacking = true,
```

A player facing a draw two can play their own draw two instead of drawing, passing the
total on. Four stacked draw twos means somebody picks up eight.

On by default because it is how most people play, and because it is what makes a draw two
in hand worth holding. Off, a draw two always resolves immediately.

## LastCallout

```lua
LastCallout = true,
```

Whether the last-card call exists at all. On, a player with one card must call it within
`Timing.LastWindowSeconds` or take a two-card penalty.

Turn it off for a gentler game. It removes the single most punishing rule in the game, and
also the most exciting one.

## DrawUntilPlayable

```lua
DrawUntilPlayable = false,
```

Off: a player with nothing playable draws exactly one card and their turn passes.

On: they keep drawing until they can play. Turns take longer and hands get bigger, but a
player is never stuck watching their turn go by having done nothing.

Off is the faster game. On is the one that feels less arbitrary to a new player.

## StrictWildFour

```lua
StrictWildFour = false,
```

Off: a wild draw four may be played at any time.

On: it may only be played when the player genuinely has no card matching the current
colour, which is the official rule and the one people argue about.

Turning it on removes the strongest bluff in the game. That is a real design decision:
strict is fairer, loose is more fun.

<Callout type="warning" title="Two switches are configured but not implemented">

`SevenZero` and `JumpIn` appear in the settings file and are read by nothing. Setting them
to `true` changes no behaviour.

They are placeholders for two common house rules: swapping hands on a seven and passing
hands on a zero, and playing an identical card out of turn. If you need them, they are
additions to `Shared/CC/Rules.luau` and the session's turn handling, not a setting flip.

</Callout>

## Choosing a set

Three combinations worth considering:

| For | Settings |
|---|---|
| The game most people expect | `Stacking`, `LastCallout` on, the rest off. The shipped default |
| A friendlier game | Add `DrawUntilPlayable`, turn `LastCallout` off |
| A strict game | Add `StrictWildFour`, keep everything else as shipped |

Whatever you pick, tell your players. Every one of these switches is something somebody
will assume works the other way.
