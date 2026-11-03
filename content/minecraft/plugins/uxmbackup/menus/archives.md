---
title: Archives
order: 401
description: "The /uxmbackup menu window: every recent run, its state and size, and verifying one."
icon: archive
---

`/uxmbackup menu` (`uxmbackup.menu`). Six rows: the recent runs inside a grey ring.

## Slots

| Slot | Tile | Click |
|---|---|---|
| 10 to 16, 19 to 25, 28 to 34, 37 to 43 | One run each, newest first: a chest, or a clock while it runs | Left: verify it. Without `uxmbackup.verify`, refused with a bass note |
| 22 | Barrier, `Nothing yet`, when no set has finished | Nothing |
| 48, 50 | Arrows | Pages |
| The ring | Grey pane | Nothing |

The window reads the `recent` (112) runs of the history.

## A run

| Line | Finished | In flight |
|---|---|---|
| Title | The archive id | The archive id |
| Crumb | `Archive` | `In flight` |
| Description | `One finished run of a set. Everything it holds is listed in its manifest.` | `This run has not finished. The console follows it.` |
| Set, Size, Kept in | The set, the size, the storages | the same |
| Taken / Started | When | When |
| State | `whole` (good), `partial` (warn), `failed` (bad) | `running` |
| Action | `Click to check it against its manifest` | `Wait for it to finish` |

The title colours are `role-done`, `role-partial`, `role-running` and `role-failed` in the window's `placeholders`.

Related: [Verifying](../features/verifying.md), [Menu files](../config/menu-files.md)
