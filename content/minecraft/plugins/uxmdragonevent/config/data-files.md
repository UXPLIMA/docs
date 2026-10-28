---
title: Data files
order: 52
description: The files the plugin writes for itself, when it writes them, and what to back up.
icon: database
---

The plugin writes these files itself. Edit them only with the server stopped: a running server
writes over your changes.

## locations.yml

The spawn points:

| Entry | Set by | Stored as |
|---|---|---|
| `lobby-spawn` | `/dragon setlobbyspawn` | World, position and facing |
| `custom-world-spawn` | `/dragon setcustomspawn` | Position and facing only |
| `custom-dragon-spawn` | `/dragon setdragonspawn` | Position and facing only |

The two event-world points carry no world name, because the event world is a fresh copy every time.

The file is written on `/dragon save`, every 10 minutes, when an event ends, on `/dragon reload` and
on shutdown. Set the points with the commands rather than by hand: a position that is slightly off
puts players inside a block.

## autostart_data.yml

The timetable's record:

| Entry | Holds |
|---|---|
| `next` | When the next scheduled event starts |
| `last-run` | When the last scheduled event started |
| `months.<Month>.run-times` | How many scheduled events ran that month, for `max-per-month` |
| `months.<Month>.days.<day>.doneTimes` | The times that already ran on that day, so a restart in the same minute does not start the event twice |

It is read on startup, so a restart keeps the monthly count. To start the count over, stop the
server, delete the file, and start again; the plugin builds a new one from `auto-start` in
`config.yml`.

## data/stats.yml

Every player's statistics:

```yaml
<uuid>:
  name: PlayerName
  dragon_kill: 3.0
  dragon_damage: 15420.5
  death: 7.0
  win: 2.0
  lose: 1.0
```

The name is kept beside the UUID so the leaderboards can show players who are offline. The file is
written every 10 minutes, when an event ends, when a player leaves the server, on `/dragon save` and
on shutdown.

To change a player's statistics: stop the server, edit, start it again, then run `/dragon refresh`.

## data/license.cache

Written by the license check. Leave it alone.

## temp-worlds

With a custom world and no world plugin, the copy of the map used for the running event lives in
`plugins/uxmDragonEvent/temp-worlds/`. It is deleted and made again before every event; there is
nothing in it to keep.

## Backups

Back up the whole plugin folder. `data/stats.yml` is the only copy of every statistic and every
all-time leaderboard; there is no database to fall back on. `locations.yml` is a few minutes of setup
you would otherwise redo.

Run `/dragon save` before copying the folder from a running server, or copy it with the server
stopped.

<Callout type="warning" title="Losing stats.yml loses every leaderboard">

The event world is meant to be thrown away and rebuilt. `data/stats.yml` is not: losing it wipes
every player's history and every all-time leaderboard, and nothing can rebuild it.

</Callout>
