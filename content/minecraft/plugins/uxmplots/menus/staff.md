---
title: Staff windows
order: 407
description: The report queue, the inactive list and the staff bench.
icon: user-cog
---

## Report queue

`/uxmplots reports` (`uxmplots.admin.reports`). Open reports in slots 0 to 35. Left click chooses one, right click goes
to the plot. Then decide it:

| Slot | Decision |
|---|---|
| 38 | `valid`: something was wrong and it has been dealt with; the reporter is paid |
| 40 | `cleaned`: something was wrong and you tidied it; the reporter is paid |
| 42 | `unfounded`: nothing was wrong; nobody is paid or punished |
| 49 | Opens the [inactive list](#inactive) |
| 51 | Opens the [bench](#bench) |

Tokens: `%report_plot%`, `%report_who%`, `%report_reason%`, `%report_when%`, `%reports_open%`, `%report_chosen%`.
Conditions: `plots:queue-empty`, `plots:holds:<node>`.

## Inactive

`/uxmplots inactive`. Every plot past `expiry.expire-after-days`. Left click goes to look; right click gives it back,
asked twice, with a picture kept. A barrier says when this server expires nothing. Tokens: `%inactive_plot%`,
`%inactive_owner%`, `%inactive_away%`, `%inactive_since%`, `%inactive_count%`.

## Bench

`/uxmplots staff` over the plot you stand on.

| Slot | Button | Node |
|---|---|---|
| 4 | Switch redstone back on | `uxmplots.admin.redstone` |
| 10 | Reset, asked twice | `uxmplots.admin.reset` |
| 12 | Snapshot | `uxmplots.admin.snapshot` |
| 14 | Take the plot back, the blocks staying, asked twice | `uxmplots.admin.delete` |
| 16 | Lift or put back a contest lock | `uxmplots.admin.unlock` |
| 22 | Back to the queue | `uxmplots.admin.reports` |

Each button is drawn and answers only for a holder of its node (`plots:holds:<node>`).

Related: [Expiry and staff](../features/expiry-and-staff.md)
