---
title: Leave
order: 505
description: The confirmation that shows the leave cost and what the archive keeps.
icon: log-out
---

`menus/quit.conf`: three rows. It shows the number before it is paid; a job left by accident is a ticket, and this
window stops one.

| Slot | Item | Click |
|---|---|---|
| 11 | Red concrete, leave, with the cost | Leaves and pays |
| 13 | The job's icon: the level given up and the experience the archive keeps | |
| 15 | Lime concrete, stay | Back to the job |

Tokens: `%job_name%`, `%job_level%`, `%job_experience%`, `%job_leave_cost%`. Action `jobs:leave`.

`employment.confirm-before-leaving` does not touch this window: it is a confirmation the player opened on purpose.

Related: [Joining and leaving](../features/joining-and-leaving.md)
