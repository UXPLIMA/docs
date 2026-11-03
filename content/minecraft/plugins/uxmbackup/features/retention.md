---
title: Retention
order: 303
description: The three limits per storage, and the archive that is never removed.
icon: trash-2
---

Every storage has three retention limits. Whichever is reached first removes the oldest archives:

| Key | Meaning |
|---|---|
| `keep-count` | How many archives stay |
| `keep-size-mb` | How much space they may take together |
| `keep-days` | How old an archive may get |

`0` means the limit is off, not "keep none". **The newest archive is never removed**, whatever the limits say, so a
server that was off for a month still has its last copy when it starts again.

Retention is applied after each run and on `/uxmbackup prune`, which reports per storage how many were removed and
kept. A storage that cannot be pruned is named with the reason.

The shipped `local` storage keeps 10 archives for at most 30 days.

Related: [Storages](storages.md)
