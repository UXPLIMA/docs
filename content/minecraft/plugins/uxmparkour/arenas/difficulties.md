---
title: Difficulties
order: 402
description: The range of jumps a difficulty allows, the jumps a player can physically make, and writing your own.
icon: gauge
---

A difficulty is a box of jumps: how far forward the next block may be, how far to either side, and how much higher
or lower. The generator only ever draws from the jumps inside that box that a player can actually make. Difficulties
live in the `difficulties` block of [`config.conf`](../config/config-conf.md#difficulties).

## The shipped difficulties

```hocon
difficulties {
  easy   { min-forward = 2, max-forward = 3, max-sideways = 1, min-rise = -1, max-rise = 1 }
  normal { min-forward = 2, max-forward = 4, max-sideways = 2, min-rise = -1, max-rise = 1 }
  hard   { min-forward = 3, max-forward = 5, max-sideways = 3, min-rise = -2, max-rise = 1 }
}
```

| Difficulty | Jumps it can draw | Longest gap forward | Drops up to |
|---|---|---|---|
| `easy` | 16 | 3 | 1 |
| `normal` | 32 | 4 | 1 |
| `hard` | 43 | 5 | 2 |

`run.default-difficulty` (`normal`) is the difficulty of a player who has chosen none, in an arena whose own
difficulty no longer exists.

## The keys

| Key | Meaning | Rule |
|---|---|---|
| `min-forward` | The shortest gap along the heading | At least 1 |
| `max-forward` | The longest gap along the heading | Not below `min-forward` |
| `max-sideways` | How far to either side of the line the landing may be | Not negative; `0` is a straight line |
| `min-rise` | The lowest the landing may be, negative for down | |
| `max-rise` | The highest the landing may be | Not below `min-rise` |

The id on the left is what a player types after `/parkour difficulty` and what an arena names. It is matched in any
case.

## What a player can make

Every jump in the box is checked against what a player can reach with a sprint jump and no potion, measured between
the tops of the two blocks. The gap is the straight-line distance across, so a jump two forward and two sideways is
a gap of about 2.8.

| Height change | Furthest gap |
|---|---|
| Up 1 | 3 blocks |
| Level | 4 blocks |
| Down 1 | 5 blocks |
| Down 2 or more | 6 blocks |
| Up 2 or more | none: a player cannot climb two blocks in one jump |

A gap under 2 is a step, not a jump, and is never drawn. A jump outside these limits is dropped from the difficulty
before the generator sees it, so no course has an impossible jump. `max-rise = 2` is accepted, and the rises of two
simply never appear.

A difficulty whose box holds no possible jump at all, such as `min-forward = 5` with `max-rise = 1` and
`min-rise = 1`, stops the reload with its id and the reason, and the last working configuration stays.

## Writing your own

```hocon
difficulties {
  # Long straight sprints, no height change: a speed course.
  sprint { min-forward = 4, max-forward = 4, max-sideways = 0, min-rise = 0, max-rise = 0 }

  # Short hops that wander: a course for new players.
  stroll { min-forward = 2, max-forward = 2, max-sideways = 1, min-rise = 0, max-rise = 1 }
}
```

Run `/uxmparkour reload`. The new ids are offered by `/parkour difficulty` and can be set on an arena. Players who
chose a difficulty you removed fall back to the arena's.

A list of difficulties is yours: an update never puts back one you deleted.

## Notes

- **A difficulty changes the jumps, not the blocks.** The blocks come from the [theme](themes.md).
- **The box of the arena still applies.** A wide `max-sideways` in a narrow arena turns the course more often, and
  a small arena runs out of room sooner on `hard`.
- **The fall rule is separate.** `run.max-fall` decides how far below the last block a player may drop before the
  run ends: see [Runs](runs.md#how-a-run-ends).

Related: [Themes](themes.md), [Arenas](overview.md), [config.conf](../config/config-conf.md#difficulties)
