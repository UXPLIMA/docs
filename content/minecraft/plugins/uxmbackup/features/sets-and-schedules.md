---
title: Sets and schedules
order: 301
description: What a set takes, the cron and period schedules, and excludes.
icon: calendar-clock
---

## A set

```hocon
sets {
  full {
    worlds   = ["world", "world_nether", "world_the_end"]
    paths    = ["plugins", "server.properties", "bukkit.yml", "spigot.yml", "paper-global.yml"]
    storages = ["local"]
    schedule = "0 4 * * *"
    excludes = []
    databases {}
  }
}
```

| Key | Meaning |
|---|---|
| `worlds` | World names. The world is saved to disk before it is read |
| `paths` | Files and folders, relative to the server folder |
| `storages` | Ids from the `storages` block the archive goes to |
| `schedule` | A cron expression or a period. Empty: only on `/uxmbackup run <set>` |
| `excludes` | Globs left out, added to the global `excludes` list, never replacing it |
| `databases` | Databases dumped through their own engine; see [Databases](databases.md) |

The shipped sets:

| Set | Takes | When |
|---|---|---|
| `full` | The three worlds, `plugins` and the server's own files | `0 4 * * *`: every night at 04:00 |
| `light` | `plugins`, leaving out `plugins/*/cache` | `6h`: every six hours |

Both go to `local`. These are examples; name your own. A set you remove from the file is not written back.

## Schedules

A schedule is either five cron fields, `minute hour day-of-month month day-of-week`, or a period such as `30m`, `6h` or
`1d`.

| Kind | Behaviour |
|---|---|
| Period | Counts from the last run |
| Cron | Fires at the clock times; does not drift with restarts |

In cron, a step counts from the first value of its field: `*/2` in the day of month is the 1st, 3rd, 5th, and `*/3` in
the month is January, April, July and October. Sunday is `0` or `7`.

| Example | Fires |
|---|---|
| `0 4 * * *` | 04:00 every day |
| `30 */6 * * *` | At half past every sixth hour |
| `0 3 * * 0` | 03:00 every Sunday |
| `0 5 1 * *` | 05:00 on the first of each month |

## Excludes

An exclude is a glob matched against the path inside the archive, without regard to case:

| Pattern | Matches |
|---|---|
| `*` | Within one folder |
| `**` | Across folders |
| `**/` | Any number of folders, including none: `**/*.lock` leaves out lock files at any depth |
| A plain name such as `logs` | That folder and everything in it |

The global `excludes` ships with `libraries`, `cache`, `logs`, `crash-reports`, `spark` and `**/*.lock`. The plugin
never backs up its own archive folder or any local storage's folder, by path, whatever it is called.

Related: [config.conf](../config/config-conf.md#sets), [A run](a-run.md)
