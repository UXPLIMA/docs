---
title: Data and backups
order: 410
description: Backups, restore, the AuraSkills import, recalculation, inspection and diagnostics.
icon: hard-drive
---

## Backups

```hocon
backups { on-shutdown = true, interval-minutes = 360, maximum-players = 0, keep = 10 }
```

A backup on every shutdown and every `interval-minutes` (0 off), into `backups/`. None runs while more than
`maximum-players` are on (0 no ceiling). `keep` automatic backups are kept. `/uxmskills backup` writes one now and
`/uxmskills restore <file>` reads one back. The file is the same either way, so a shutdown's backup is what moves a
server from SQLite to MySQL.

## Import

`/uxmskills import <folder>` reads AuraSkills' `userdata/` files, one per player. It reads their data, never their
code, and answers with what it found; a folder that does not match reads as zero.

## Changing players

`give`, `take`, `set`, `setlevel` and `addlevel` change experience and levels; `reset` and `resetall` clear a player;
`wipe confirm` clears everyone. `/uxmskills inspect <player>` reads what a player holds.

## Recalculation

`/uxmskills recalculate` rewrites every stored level from experience after a curve change.

## Diagnostics

`/uxmskills diagnose` writes `reports/diagnostic-<time>.txt`: which files loaded, what is wrong with them, how many
rows there are, and what the plugin thinks its settings are.

Related: [Admin commands](../commands/admin.md)
