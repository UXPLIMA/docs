---
title: Report
order: 411
description: How much money the jobs created over the window, and from what.
icon: bar-chart
---

`/uxmjobs report menu` (`uxmjobs.admin.report`) opens `menus/report.conf`: six rows, read from a cache.

| Slot | Item |
|---|---|
| 4 | Gold ingot: the whole window in money and experience, and how many lines |
| 9 to 44 | One paper per job per currency, largest first: paid, payments, share of the window |
| 49 | Barrier, close |

Tokens: `%report_job%`, `%report_currency%`, `%report_amount%`, `%report_payments%`, `%report_share%`,
`%report_money%`, `%report_experience%`, `%report_lines%`, `%report_age_minutes%`. List source `jobs:report`.

Related: [Report and tools](../features/report-and-tools.md)
