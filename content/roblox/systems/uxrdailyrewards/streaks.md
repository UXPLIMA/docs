---
title: Streaks and cooldowns
order: 4
description: The rolling cooldown model, when a streak resets, and what looping does.
icon: flame
---

Two numbers are saved per player:

| Field | Meaning |
|---|---|
| `LastClaimAt` | Unix time of the last successful claim, `0` for a new player |
| `StreakIndex` | Rewards claimed in the current cycle, from `0` to the ladder length |

Everything the panel shows is derived from those two by a pure function,
`Shared/DailyRewards/Streak.resolve`. It touches no `DataStore`, no player and not even
the clock: the time is passed in. That is what makes the behaviour below exact rather
than approximate.

## It is a cooldown, not a calendar

The wait is measured from the last claim, not from midnight.

Claim at 23:00 and the next reward is ready at 23:00 the following day. A player who
claims a little later each day drifts later each day. A player who claims at 22:00 and
comes back at 00:30 has waited two and a half hours, not "the next day", and is refused.

This is the standard mobile-game model and it is the one players expect. It also removes
every time zone question: there is no server day boundary to argue about.

## What resolve returns

| Field | What it is |
|---|---|
| `currentDay` | The 1-based day the player would claim next |
| `claimable` | Whether they can claim right now |
| `secondsUntilNext` | Seconds left, `0` when claimable |
| `didReset` | Whether this claim starts a fresh cycle after a miss |
| `effectiveStreakIndex` | The streak index with any reset or wrap already folded in |

## The rules, in order

**A player who has never claimed** gets day 1 immediately. `LastClaimAt` of `0` means
claimable, so a new join is always greeted with something to take.

**Inside the cooldown**, the pending day is `StreakIndex + 1` and `secondsUntilNext`
counts down. Nothing else changes.

**Once the cooldown elapses**, the day becomes claimable, and only then is the miss check
made.

## When the streak resets

With `ResetStreakOnMiss = true`, the reset fires when

```
elapsed >= ClaimIntervalSeconds * (1 + ResetAfterMissedDays)
```

At the defaults, `86400` and `1`, that is 48 hours. Under 24 hours nothing is claimable.
Between 24 and 48 hours the next day is claimable normally. Past 48 hours the next claim
is day 1 again.

| `ResetAfterMissedDays` | Grace after the cooldown | Effect |
|---|---|---|
| `0` | None | Any claim later than one full interval resets |
| `1` | One full interval | The default: one whole missed day is forgiven |
| `2` | Two full intervals | A weekend away does not cost the streak |

`ResetStreakOnMiss = false` disables it entirely. A player returning after a month
resumes at the day they were on.

A reset on a player who had claimed nothing yet is not reported as a reset, so a returning
player who never got past day 1 does not see the message.

## Looping

`LoopAfterLastDay = true` sends a player who finishes the last day back to day 1 and the
ladder continues forever.

With `false`, a finished player stops: `claimable` is `false`, `currentDay` stays on the
last day, and no cooldown is shown. They have completed the ladder and there is nothing
further to give them.

## Claim order

A successful claim does, in this order:

1. fires `onStreakReset` when this claim starts a fresh cycle after a miss
2. runs the day's `Grant`
3. advances `StreakIndex` and stamps `LastClaimAt`
4. fires `onClaim`
5. saves the profile
6. pushes the new state to the client

Two claims from the same player cannot interleave: a claim in flight makes a second one
return `busy`. A profile that has not finished loading returns `notloaded` rather than
granting from empty data.

<Callout type="danger" title="Do not shorten the interval on a live game to run an event">

Every player's next claim is measured from their own last claim, so cutting
`ClaimIntervalSeconds` in half makes everyone who claimed recently claimable at once, and
a player mid-ladder can walk the rest of it in an afternoon. Run events by changing what
`Grant` gives, not by changing the clock.

</Callout>
