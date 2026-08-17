---
title: Rules and play
order: 3
description: The eight-ball ruleset as implemented, phase by phase, plus fouls and the shot clock.
icon: scroll-text
---

Eight-ball, decided entirely on the server. The rules module is pure logic with no Roblox
API in it, and it ships with a spec, so the behaviour below is exact rather than
approximate.

## The three modes

| Mode | Who plays |
|---|---|
| Two players | The first player joins and waits; the second starts the match |
| Solo | Practice alone. Optionally without a shot clock |
| AI | Against the built-in opponent. See [The AI opponent](ai.md) |

Solo and AI matches never affect ratings or leaderboards.

## Phases

A match moves through four phases.

| Phase | Meaning |
|---|---|
| `preBreak` | Nobody has broken yet |
| `open` | The table is open: neither player owns a group |
| `assigned` | Groups are decided; each player is potting theirs |
| `eight` | The shooter has cleared their group and is on the black |

## The break

A legal break needs contact with the pack and then either a potted ball or a ball reaching
a rail.

If the break is illegal, or the cue ball scratches, the turn passes with **ball in hand**.
Otherwise the breaker continues and the table is open.

Potting on the break does not assign groups. The table stays open.

## Assigning groups

The first legal pot on an open table assigns the groups: the shooter takes the group of
the first non-black ball they potted, and the opponent takes the other.

Solids are 1 to 7, stripes are 9 to 15, and 8 is the black.

Because assignment happens on a **legal** pot, a foul that happens to sink a ball leaves
the table open.

## Fouls

Any of these is a foul, and every foul hands the opponent ball in hand:

| Foul | Meaning |
|---|---|
| Scratch | The cue ball went down |
| No contact | The cue ball hit nothing |
| Wrong ball first | The first ball struck was not from the shooter's group |
| No progress | Nothing was potted and no ball reached a rail |

The no-progress rule is what stops a player from safety-playing by nudging the cue ball a
stud and passing the turn back.

Once a shooter is on the black, the black is their legal first ball; hitting anything else
first is a wrong-ball foul.

## Ball in hand

After a foul, the incoming player places the cue ball anywhere on the table before
shooting. The placement is validated on the server.

## Winning and losing on the black

Potting the black wins **only** when all three of these hold:

1. the shooter had already cleared their group before the shot
2. no foul on the shot, no scratch, and contact was made
3. the black went into the called pocket, when a pocket was called

Otherwise potting the black is an immediate loss.

<Callout type="danger" title="A scratch on the black loses the game">

Sinking the black and the cue ball on the same shot is a loss even when the group was
cleared and the pocket was called. That is the standard rule and it is worth putting in
your own onboarding: it is the ending new players complain about.

</Callout>

## Called pockets

A call is optional in the implementation: when no pocket has been called, the pocket check
is skipped. When one has been called, the black must go into it.

If you want a stricter game, require the call in your own interface flow. The rules module
already enforces it when it is present.

## Continuing a turn

A shooter continues when they legally pot at least one ball of their own group. Potting an
opponent's ball, or nothing at all, passes the turn without being a foul in itself.

## The shot clock

```lua
Config.shotClockSeconds = 30
Config.soloShotClock = false
```

Thirty seconds per shot. Running out is treated as a foul, which hands over ball in hand
rather than skipping the turn: a player who walks away does not hold a table hostage.

`soloShotClock = false` turns it off for solo practice, which is the right default. Nobody
needs a timer against nobody.

## When a player leaves

Leaving the table or the server ends the match for the other player. Their movement is
restored, their seat is freed, and the table resets and waits for the next player.
