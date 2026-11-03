---
title: One job
order: 403
description: What a job pays at your level, your standing, its rewards and the way out.
icon: file-search
---

`/jobs menu <job>` or a left click on the board opens `menus/job.conf` on one job: six rows.

| Slot | Item | Click |
|---|---|---|
| 4 | The job's icon: level, experience, to the next level, progress | |
| 10 to 16, 19 to 25, 28 to 34 | One square per price row, drawn as its target | |
| 45 | Arrow, back | The job board |
| 48, 50 | Arrows | Pages of rows |
| 49 | Experience bottle, rewards | The [levels window](levels.md) |
| 53 | Barrier, leave, with the cost on it | The [leave window](quit.md) |

Each row reads what it pays you in money, experience and points at your level. A target with no material is drawn as
stone.

| Token | Value |
|---|---|
| `%action_key%` | The row, as `behaviour:target` |
| `%action_type%`, `%action_target%` | In words |
| `%action_icon%` | The target as a material |
| `%action_money%`, `%action_experience%`, `%action_points%` | What it pays you |
| `%job_remaining%` | Experience left to the next level |
| `%job_leave_cost%` | What leaving would cost now |

List source `jobs:actions`; action `jobs:open-levels`.

Related: [Levels](levels.md), [Jobs and prices](../features/jobs-and-prices.md)
