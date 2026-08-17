---
title: Leaderboards
order: 37
icon: trophy
---

Two different leaderboards, and it is worth keeping them apart.

## The event leaderboard

Damage dealt during the **current** event. It decides who gets the `places` rewards, and it is what
the end-of-event message shows:

```
%dragonevent_top_name_1%      the name in first place
%dragonevent_top_damage_1%    their damage
```

Places 1 upward. A place nobody occupies returns the `leaderboard-empty-entry` message — `—` by
default — rather than an empty string, so a scoreboard with three lines still has three lines when
only two people joined.

This leaderboard exists only while an event is running.

## All-time leaderboards

Built from `data/stats.yml`, across every event ever played.

```
%dragonevent_leaderboard_<board>_<field>_<place>%
```

| Board | Ranks by |
|---|---|
| `damage` | Total damage dealt to dragons |
| `kill` | Killing blows landed |
| `win` | Events won |

`<field>` is `name` or `value`. So:

```
%dragonevent_leaderboard_damage_name_1%
%dragonevent_leaderboard_damage_value_1%
%dragonevent_leaderboard_kill_name_3%
%dragonevent_leaderboard_win_value_1%
```

Damage values are formatted with two decimal places; kills and wins as whole numbers.

## Refreshing

All-time leaderboards are cached and rebuilt periodically. To force it:

```
/dragon refresh
```

Needed after editing `data/stats.yml` by hand. Under normal use the plugin keeps them current.

## Statistics behind them

`data/stats.yml` holds five values per player:

| Key | Meaning |
|---|---|
| `dragon_kill` | Killing blows |
| `dragon_damage` | Total damage |
| `death` | Deaths in events |
| `win` | Events won |
| `lose` | Events lost or timed out |

Players read their own with `/dragon stats`, formatted by the `stats-of` message.

Only `damage`, `kill` and `win` have leaderboards. Deaths and losses are recorded and shown in
`/dragon stats`, but not ranked — which is right: a death leaderboard rewards the wrong thing.

## Storage

Flat file. `data/stats.yml` is the whole store, written on `/dragon save` and on shutdown.

Back it up with the rest of the plugin folder. There is no database backend, so this one file is
every statistic and every all-time leaderboard you have.

<Callout type="note" title="Editing stats.yml">

Stop the server, edit, start, then `/dragon refresh`. Editing it live means the plugin overwrites
your changes on the next save.

</Callout>
