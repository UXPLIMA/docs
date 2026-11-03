---
title: Boosters
order: 407
description: Permission boosters, the timed boost, scheduled boosters, and how two combine.
icon: rocket
---

Three kinds and no more. An item that raises earnings was offered and refused: it adds an inventory to police, a
duplication surface and a fourth place to look when a number is wrong.

## Permission boosters

`boosters.conf`:

```hocon
combine = "highest"

permission {
  vip {
    node       = "uxmjobs.booster.vip"
    multiplier = "1.25"
    jobs       = []
    currencies = ["money"]
  }
}
```

A player holding the node earns the multiplier. `jobs` and `currencies` narrow it; empty covers all. Nothing ships here:
a shipped node would be a rank this plugin knows the name of.

## The timed boost

`/uxmjobs boost start <multiplier> <minutes>` starts a server-wide boost for every job and currency, broadcast to the
server and kept across a restart. `/uxmjobs boost stop` ends it.

## Scheduled boosters

`schedule.conf`:

```hocon
boosters {
  weekend {
    enabled    = false
    multiplier = "1.5"
    days       = ["saturday", "sunday"]
    start      = "00:00"
    end        = "23:59"
    jobs       = []
    currencies = []
  }
}
```

| Key | Meaning |
|---|---|
| `days` | `monday` to `sunday`, in any case |
| `start`, `end` | 24 hour times; a window that ends before it starts runs through midnight (22:00 to 02:00) |
| `jobs`, `currencies` | Empty covers all |
| `multiplier` | An expression |
| `broadcast` | A message key sent when it opens; leave it out for a quiet boost |

Every window is read in the server machine's time zone. `weekend` (1.5) and `evening` (1.25, weekdays 19:00 to 22:00)
ship off.

## Combining

| `combine` | Two boosters of 2 and 1.5 give |
|---|---|
| `highest` | 2: only the largest counts |
| `summed` | 2.5: the extra part of each is added |

A server that sells a permanent multiplier and runs a weekend event usually wants them summed; one that hands them out
freely wants only the best.

`/jobs boosters` and the [boosters window](../menus/boosters.md) show what applies; `%uxmjobs_booster_<currency>%` gives
the combined number.

Related: [Payments](payments.md), [boosters.conf](../config/boosters-conf.md)
