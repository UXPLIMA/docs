---
title: Concepts
order: 14
description: How an event starts and ends, rebirths, a player's part in the event, damage and the five statistics.
icon: lightbulb
---

## The event

One event runs at a time. It starts on the [schedule](../features/scheduling.md) or with
`/dragon start`:

1. The event world is rebuilt.
2. The dragon spawns, the start message, title and sound go to the whole server, the `commands.start`
   commands run, and Discord is told.
3. Players join through the entrance for the next `settings.dragon-event-time` minutes.

It ends in one of three ways:

| How it ends | Rewards | Statistics |
|---|---|---|
| The dragon is killed `required-kills` times | Given | Everyone in the event gets a win |
| The time runs out | None. Everyone is told `timed-out` | Everyone in the event gets a loss |
| `/dragon stop`, `/dragon reload`, or the server stopping | None. Everyone is told `force-stop` | Nothing recorded |

In every case, the players in the event are sent to the lobby spawn.

## Rebirths

```yaml
dragon:
  required-kills: 2
```

The dragon has to die more than once. Each death but the last is a **rebirth**: the dragon comes back
with full health, the player who landed the killing blow gets the `each-last-hit` reward, and the
fight goes on. The last death ends the event and pays out the leaderboard.

One kill is over in a minute on a busy server. Two or three give the fight rounds, and the
killing-blow rewards give players something to compete for in each one.

## A player's part in the event

From the moment a player joins until they leave, the plugin counts the damage they deal to the
dragon. A player leaves by `/dragon leave`, by dying, by falling into the void, or by quitting the
server. Whether their damage is kept for when they come back is up to `keep-stats-on-leave`; see
[Joining](../features/joining.md#coming-back).

Only players in the event when it ends are ranked, rewarded and given a win or a loss.

## Damage and the leaderboard

Every hit a player in the event lands on the dragon is added to their damage. At the end, players
are ranked by damage: the `places` rewards go to the top of that ranking and `every-other-player` to
the rest.

Damage decides the ranking rather than kills, so a dragon finished by one lucky arrow does not pay
the archer the same as the players who wore it down for ten minutes. The lucky arrow still gets the
killing-blow reward.

## Statistics

Five statistics are kept per player across every event:

| Statistic | Counts |
|---|---|
| `dragon_kill` | Killing blows, one for every death of the dragon, rebirths included |
| `dragon_damage` | All damage dealt to event dragons |
| `death` | Deaths during events |
| `win` | Events the player was in when the dragon was killed for the last time |
| `lose` | Events the player was in when the time ran out |

A win means being there at the end of a successful event, whatever the damage. Players see their own
with `/dragon stats`. The all-time [leaderboards](../features/leaderboards.md) rank damage, killing
blows and wins.

## Worlds

| World | Role |
|---|---|
| `lobby-world-name` | Holds the entrance region |
| The lobby spawn | Where players are sent when they leave or the event ends. Set with `/dragon setlobbyspawn` |
| `event-world-name` | Where the fight happens. Built by the plugin and rebuilt before every event |

See [Worlds and the border](../features/worlds.md).

## End rules

Inside the event world the plugin overrides vanilla behaviour: PvP, damage, keeping items, block
breaking, elytra, potions and more. The defaults make the event a co-operative fight with no risk.
See [End rules](../features/end-rules.md).
