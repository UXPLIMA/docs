---
title: Throws and landings
order: 403
description: When a pad throws, the direction, the cooldown, one throw per arrival, and the fall grace.
icon: move-up-right
---

## When a pad throws

A pad throws a player when all of these hold:

| Condition | Notes |
|---|---|
| The player's feet reached a new block | Standing still or turning on a pad throws nobody |
| They stand on a pad | A set pad at their feet or under them, or a type matched at their feet or under them |
| They are not flying, gliding, riding or in spectator | |
| It is not the pad that last threw them | Until they stand on ground that is not that pad |
| They hold the type's `permission`, if it has one | |
| `cooldown-ms` has passed since their last throw | 750 as shipped |

A pad that does not throw says nothing: a pad is part of the floor.

## The direction

| Pad | Throws toward |
|---|---|
| Matched by its blocks | The way the stepping player faces |
| Set on a block | The way the operator faced when they set it |

The throw sets the player's speed: `forward` along that direction, flat, and `upward` up. A type with `forward = 0`
throws straight up whatever the facing. A throw replaces the speed the player had, so running onto a pad adds nothing.

## One throw per arrival

After a throw, the pad remembers who it threw. Landing back on the same pad without having stood anywhere else does
not throw again, so a straight-up pad does not keep a player bouncing. Stepping off onto other ground ends the
arrival; walking back on throws again.

## The cooldown

```hocon
cooldown-ms = 750
```

Milliseconds before any pad throws the same player again. A player crossing a plate makes several moves inside it,
and without the wait each could throw. Raise it for a chain of pads meant to be walked, not run. `0` is no wait.

## The landing

```hocon
fall-damage {
  grace-ms = 10000
}
```

After a throw, the first fall damage within `grace-ms` is cancelled. The grace is spent on that fall, so a player who
lands, walks off a ledge and falls again takes the second fall. A new throw starts a new grace. `0` leaves fall damage
alone. A death or a quit drops any grace a player held.

Related: [Pad types](types.md), [Effects](effects.md), [config.conf](../config/config-conf.md)
