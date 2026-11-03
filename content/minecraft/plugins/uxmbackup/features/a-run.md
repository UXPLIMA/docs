---
title: A run
order: 304
description: Everything a run does, from the free-space check to the last storage, and the shutdown backup.
icon: play
---

1. **Refusals.** A run is refused when another is in flight, or when the disk would be left with less than
   `free-space-floor-mb` (2048) free. A backup plugin that fills the disk takes down the server it exists to protect.
2. **Participants.** Every plugin that registered with uxmLib is told to write its state to disk, and they are given
   `participants.timeout-seconds` (30) together. A plugin that will not finish is named in the console and does not
   hold the backup open.
3. **Worlds** are saved to disk before they are read.
4. **The archive** is built in `work-folder` (`work`) at `compression-level` (5). A world is mostly region files,
   already compressed, so a high level costs processor time and saves little; raise it for a set of configuration
   files.
5. **Databases** are dumped into the archive; see [Databases](databases.md).
6. **Progress** is written to the console every `progress.every-seconds` (15), in bytes.
7. **Storages** each receive the archive. `done` when all did, `partial` when some did.
8. **Retention** runs, and the moment's effects and the Discord message go out.

`work/` belongs to the plugin and is the only place it deletes.

## When the server stops

```hocon
shutdown {
  enabled = false
  set     = "light"
  limit-seconds = 120
}
```

With `enabled = true` the named set runs as the server stops, for at most `limit-seconds`: a backup that will not
finish never holds a restart open forever.

Related: [config.conf](../config/config-conf.md#run), [Alerts](alerts.md)
