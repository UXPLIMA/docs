---
title: Leaderboards
order: 37
description: The damage ranking of the running event, the all-time leaderboards, and when they update.
icon: trophy
---

There are two kinds of leaderboard, and they answer different questions.

## The event leaderboard

Damage dealt in the **running** event, by the players in it right now. It decides who gets the
`places` rewards, and it is what the end message shows.

```
%dragonevent_top_name_1%      the name in first place
%dragonevent_top_damage_1%    their damage
```

It is updated every second. A player who leaves drops off it; with `keep-stats-on-leave` they come
back with their damage when they rejoin.

A place nobody holds shows the `leaderboard-empty-entry` message, a grey dash by default, so a
scoreboard with three lines keeps three lines when only two people joined. With no event running,
every place shows it.

## All-time leaderboards

Built from the saved statistics of every player who ever took part.

```
%dragonevent_leaderboard_<board>_<field>_<place>%
```

| Board | Ranks by |
|---|---|
| `damage` | Total damage dealt to event dragons |
| `kill` | Killing blows |
| `win` | Events won |

`<field>` is `name` or `value`, and `<place>` counts from 1:

```
%dragonevent_leaderboard_damage_name_1%
%dragonevent_leaderboard_damage_value_1%
%dragonevent_leaderboard_kill_name_3%
%dragonevent_leaderboard_win_value_1%
```

Damage is shown with up to two decimals, killing blows and wins as whole numbers.

Deaths and losses are counted and shown in `/dragon stats`, but not ranked: a death leaderboard
would reward the wrong thing.

## When they update

The all-time leaderboards are read from `data/stats.yml`, not from the running server. The plugin
saves the statistics and rebuilds the leaderboards every 10 minutes, so a finished event can take
up to 10 minutes to show up on them.

To update them now:

```
/dragon save
/dragon refresh
```

`/dragon refresh` alone rebuilds from the last save, so run `/dragon save` first.

Statistics are saved every 10 minutes, when an event ends, when a player leaves the server,
on `/dragon save`, and on shutdown.

## Storage

`data/stats.yml` is the whole store: every statistic and every all-time leaderboard comes from that
one file. There is no database option. See [Data files](../config/data-files.md#datastatsyml).
