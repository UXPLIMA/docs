---
title: Job board
order: 402
description: "The window /jobs opens: every job, to open, join or leave."
icon: briefcase
---

`/jobs` opens `menus/jobs.conf`: five rows, with a page turn sound.

| Slot | Item | Click |
|---|---|---|
| 10 to 16, 19 to 25, 28 to 34 | Each job, drawn with its `icon` | Left: [open it](job.md). Right: join it; on a job you hold, the [leave window](quit.md) |
| 39 | Arrow, previous | |
| 40 | Barrier, close | |
| 41 | Arrow, next | |

Each tile shows the job's name, description, your level and its places: a job with a seat limit reads
`menu.jobs.entry.limited`, one without `menu.jobs.entry.unlimited`.

| Token | Value |
|---|---|
| `%job%` | The id |
| `%job_name%`, `%job_description%` | In the viewer's language |
| `%job_icon%` | The material its file names |
| `%job_level%` | The viewer's level, or 0 |
| `%job_progress%` | Percent through the level |
| `%job_seats%`, `%job_seats_left%` | Places in all (0 for no limit), and free (empty with no limit) |
| `%job_seats_state%` | `limited` or `unlimited` |
| `%job_held%`, `%job_state%` | `true` when held; `held` or `free` |
| `%player_jobs%`, `%player_points%` | How many jobs the viewer holds, and their points |

| Name | Kind | Does |
|---|---|---|
| `jobs:all` | List source | Every job |
| `jobs:held`, `jobs:can-join` | Condition | The viewer works the job; does not |
| `jobs:join` | Action | Signs on to the clicked job |
| `jobs:open-job`, `jobs:open-quit` | Action | Opens the job, or its leave window |

Related: [One job](job.md), [Joining and leaving](../features/joining-and-leaving.md)
