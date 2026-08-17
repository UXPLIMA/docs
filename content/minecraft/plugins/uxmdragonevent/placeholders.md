---
title: Placeholders
order: 60
description: The event, both leaderboards and the schedule, as placeholders.
icon: braces
---

The PlaceholderAPI expansion registers under the identifier `DragonEvent`, so every placeholder is
written `%dragonevent_...%`. Names are not case sensitive.

## The current event

| Placeholder | Returns |
|---|---|
| `%dragonevent_remaining_game%` | Time left in the event, formatted by `ingame-remaining-time-format` |
| `%dragonevent_current_health%` | The dragon's current health, two decimals |
| `%dragonevent_rebirths%` | Kills still required |
| `%dragonevent_required_kills%` | Kills required in total |
| `%dragonevent_players%` | How many players are in the event |
| `%dragonevent_health%` | Same as `rebirths` (kept for compatibility) |

With no event running, `remaining_game` returns `0m` and `current_health` and `rebirths` return
empty strings. Guard for that on a scoreboard that is always visible.

`%dragonevent_current_health%` returns `200` if the dragon has not spawned yet.

<Callout type="note" title="health does not mean health">

`%dragonevent_health%` returns the remaining **kill count**, not hit points: it is an older name for
what `rebirths` returns. For the dragon's actual health, use `%dragonevent_current_health%`.

</Callout>

## The event leaderboard

Damage in the current event:

```
%dragonevent_top_name_<place>%
%dragonevent_top_damage_<place>%
```

Places count from 1. An empty place returns the `leaderboard-empty-entry` message rather than
nothing.

The default `end` message uses places 1 to 3; there is no cap.

## All-time leaderboards

```
%dragonevent_leaderboard_<board>_<field>_<place>%
```

| Part | Values |
|---|---|
| `<board>` | `damage`, `kill`, `win` |
| `<field>` | `name`, `value` |
| `<place>` | 1 upward |

```
%dragonevent_leaderboard_damage_name_1%
%dragonevent_leaderboard_damage_value_1%
%dragonevent_leaderboard_kill_name_5%
%dragonevent_leaderboard_win_value_2%
```

Damage values carry two decimals; kills and wins are whole numbers. Rebuilt with `/dragon refresh`.

## The schedule

| Placeholder | Returns |
|---|---|
| `%dragonevent_remaining%` | Time until the next scheduled event, formatted by `remaining-time-format` |
| `%dragonevent_next%` | When the next one is, formatted by `next-event-time-format` |
| `%dragonevent_last%` | When the last one was |

These require `auto-start.enabled`. Without a schedule there is no next event to report.

## Where to use them

The plugin resolves placeholders in its own configuration: the dragon's nametag, the boss bar name,
the messages and the Discord embeds all accept them:

```yaml
dragon:
  name: "<#00E5FF>Dragon Health: <#FF5555>%dragonevent_current_health%♥"

boss-bar:
  name: "Remaining Rebirths: %dragonevent_rebirths%"
```

Outside the plugin, anywhere PlaceholderAPI reaches. `%dragonevent_remaining%` on a spawn hologram
or a scoreboard is the single most useful one: an event nobody knows is coming is an event nobody
attends.
