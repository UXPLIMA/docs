---
title: Placeholders
order: 60
description: The running event, both leaderboards and the schedule, as PlaceholderAPI placeholders.
icon: braces
---

The PlaceholderAPI expansion is built in and registers as `dragonevent`, so every placeholder is
written `%dragonevent_...%`. Names are not case sensitive.

## The running event

| Placeholder | Returns | With no event running |
|---|---|---|
| `%dragonevent_remaining_game%` | Time left, formatted by `ingame-remaining-time-format` | `0m` |
| `%dragonevent_current_health%` | The dragon's health, up to two decimals | Empty |
| `%dragonevent_rebirths%` | Kills still needed to end the event, the current dragon included | Empty |
| `%dragonevent_required_kills%` | Kills needed in total | The configured `required-kills` |
| `%dragonevent_players%` | Players in the event | `0` |
| `%dragonevent_health%` | Same as `rebirths`, kept for older setups | `0` |

`remaining_game` shows whole minutes, `14m` with 14 minutes and some seconds left, and switches to
seconds in the last minute.

`current_health` shows `200` in the moment between the event starting and the dragon appearing,
whatever its real health will be.

<Callout type="note" title="health is not the dragon's health">

`%dragonevent_health%` returns the kills still needed, like `rebirths`. For the dragon's hit points,
use `%dragonevent_current_health%`.

</Callout>

On a scoreboard that is always visible, plan for the empty values: show the event lines only while
an event runs, or use a placeholder that has a value either way.

## The event leaderboard

```
%dragonevent_top_name_<place>%
%dragonevent_top_damage_<place>%
```

Damage in the running event, places counting from 1, with no upper limit. An empty place, or any
place with no event running, returns the `leaderboard-empty-entry` message. See
[Leaderboards](features/leaderboards.md#the-event-leaderboard).

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

Updated every 10 minutes, or with `/dragon save` and `/dragon refresh`. See
[Leaderboards](features/leaderboards.md#when-they-update).

## The schedule

| Placeholder | Returns |
|---|---|
| `%dragonevent_remaining%` | Time until the next scheduled event, formatted by `remaining-time-format`. `N/A` while no next event is known |
| `%dragonevent_next%` | When the next scheduled event starts, formatted by `next-event-time-format` |
| `%dragonevent_last%` | When the last scheduled event started. Shows a date in 1970 until the first one has run |

All three need `auto-start.enabled: true` and return an empty value without it. Times are shown in
`auto-start.timezone`. An event started with `/dragon start` does not change `last`.

## Where to use them

The plugin reads placeholders in its own texts: the dragon's name, the boss bar, the messages, the
titles, the rewards and the Discord messages all accept them.

```yaml
dragon:
  name: "<#00E5FF>Dragon Health: <#FF5555>%dragonevent_current_health%♥"

boss-bar:
  name: "Remaining Rebirths: %dragonevent_rebirths%"
```

Outside the plugin, they work anywhere PlaceholderAPI reaches. `%dragonevent_remaining%` on a spawn
hologram or a scoreboard is the most useful one: an event nobody knows is coming is an event nobody
attends.
