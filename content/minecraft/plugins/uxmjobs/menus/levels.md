---
title: Levels
order: 504
description: The reward table of one job, marked by where you stand.
icon: list-ordered
---

The bottle in a job opens `menus/levels.conf`: six rows, one tile per reward, drawn with the reward's `icon`.

| Slot | Item | Click |
|---|---|---|
| 10 to 16, 19 to 25, 28 to 34 | A reward | |
| 45 | Arrow, back | The job |
| 48, 50 | Arrows | |
| 49 | Barrier, close | |

| `%reward_state%` | Means |
|---|---|
| `unlocked` | Reached, and given (an automatic reward) |
| `unclaimed` | Reached, written `automatic = false`: "Waiting for you to claim it" |
| `in-progress` | It is the very next level |
| `locked` | Further off |

Each state reads its own block, `menu.levels.entry.<state>`. Tokens: `%reward_level%`, `%reward_icon%`,
`%reward_state%`. List source `jobs:rewards`. No click claims an `unclaimed` reward in this build; see
[Rewards and titles](../features/rewards-and-titles.md).

Related: [One job](job.md)
