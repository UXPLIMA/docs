---
title: Data files
order: 52
description: The three files the plugin writes, and what to back up.
icon: database
---

Three files the plugin writes and you should not edit while the server is running.

## locations.yml

The spawn points, written by `/dragon save`:

- the lobby spawn, from `/dragon setlobbyspawn`
- the custom world's player spawn, from `/dragon setcustomspawn`
- the custom world's dragon spawn, from `/dragon setdragonspawn`

An event refuses to start without the ones it needs, with `spawn-not-set` or `event-spawn-not-set`.
`/dragon spawndebug` shows what the plugin currently holds.

Set locations with the commands rather than editing this file. The serialised format includes the
world, the coordinates and the facing, and a hand-written entry that is slightly wrong puts players
inside a block.

## autostart_data.yml

The schedule's state — when the last event ran and when the next one is due. Written by the plugin,
read on startup so a restart does not reset the timetable or the monthly count.

Delete it to reset the schedule from scratch; the plugin rebuilds it from `auto-start` in
`config.yml`.

## data/stats.yml

Every player's persistent statistics:

```yaml
<uuid>:
  name: PlayerName
  dragon_kill: 3
  dragon_damage: 15420.5
  death: 7
  win: 2
  lose: 1
```

Written on `/dragon save` and on shutdown. The name is stored alongside the UUID so leaderboards can
show a name for a player who is offline.

To edit it: stop the server, edit, start, then `/dragon refresh` to rebuild the leaderboard cache.

## Backups

Back up the whole plugin folder. `data/stats.yml` is the only copy of every all-time statistic —
there is no database backend to fall back on — and `locations.yml` is a few minutes of setup you
would otherwise redo.

`/dragon save` before a manual restart makes sure both are current.

<Callout type="warning" title="A reset event world is not a backup problem, a lost stats.yml is">

The event world is meant to be thrown away and rebuilt. `data/stats.yml` is not: losing it wipes
every leaderboard and every player's history, and nothing regenerates it.

</Callout>
