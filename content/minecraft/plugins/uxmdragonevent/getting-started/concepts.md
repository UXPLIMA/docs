---
title: Concepts
order: 13
description: Events, rebirths, sessions, damage, statistics and the three worlds.
icon: lightbulb
---

## The event

One event runs at a time. It starts on the schedule or with `/dragon start`, lasts
`settings.dragon-event-time` minutes, and ends when the dragon has been killed the required number of
times — or when the clock runs out, in which case nobody wins.

## Rebirths

```yaml
dragon:
  required-kills: 2
```

The dragon has to die more than once. Each death but the last is a **rebirth**: the dragon respawns,
the last player to hit it gets the `each-last-hit` reward, and the fight continues. The final kill
ends the event and pays out the leaderboard.

This is what turns a dragon fight into an event. One kill is over in a minute with enough players;
two or three means the fight has phases, and the last-hit rewards give people something to compete
for in each one.

## The session

A player who joins gets a session: their damage, kills and deaths for this event. It ends when the
event does, or when they leave.

```yaml
settings:
  keep-stats-on-leave: true
  auto-join-after-leaving: true
```

`keep-stats-on-leave` means a player who disconnects and comes back keeps the damage they had done —
without it, a crash costs them the event. `auto-join-after-leaving` puts them straight back in on
rejoin rather than making them find the portal again.

## Damage and the leaderboard

Every point of damage dealt to the dragon is recorded against the player who dealt it. At the end,
the top three by damage get the `places` rewards and everyone else gets `every-other-player`.

Damage is the metric rather than kills because a dragon killed by one lucky arrow should not pay the
same as one worn down over ten minutes.

## Statistics

Five persistent statistics survive the event and accumulate across all of them:

| Statistic | Meaning |
|---|---|
| `dragon_kill` | Killing blows landed |
| `dragon_damage` | Total damage dealt |
| `death` | Deaths in events |
| `win` | Events finished on the winning side |
| `lose` | Events lost or timed out |

Players see their own with `/dragon stats`. They are stored in `data/stats.yml` and feed the
all-time [leaderboards](../features/leaderboards.md).

## Worlds

Three worlds matter:

| Setting | Role |
|---|---|
| `lobby-world-name` | Where players return to |
| `event-world-name` | Where the fight happens |
| `end-portals.world-list` | Which worlds' end portals lead to the event |

The event world is reset by the plugin. Everything else is left alone.

## End rules

Inside the event world, the plugin overrides vanilla behaviour: PvP, damage, keepInventory, block
breaking, elytra, potions and about a dozen others. See [End rules](../features/end-rules.md).

The defaults make the event a co-operative fight — no PvP, no player damage, items kept on death —
which is the shape most servers want. Every one of them can be turned the other way.
