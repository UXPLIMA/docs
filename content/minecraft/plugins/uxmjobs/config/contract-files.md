---
title: Contract files
order: 605
description: The keys of contracts/<id>.conf.
icon: scroll
---

One contract per file under `contracts/`; the file name is the id.

| Key | Meaning |
|---|---|
| `name`, `description` | Per language, as a job's |
| `icon` | The material the windows draw |
| `job` | The job it is signed for |
| `cost` | An expression |
| `currency` | What the cost is taken in; empty is the order of `economy.conf` |
| `duration-hours` | How long it runs |
| `slots` | How many may hold it; `0` for no limit |
| `requirements` | Condition lines to sign |
| `cancel-cooldown-minutes` | How long before it may be cancelled |
| `working-hours` | `<day> = ["HH:MM-HH:MM", ...]` |
| `in-shift`, `out-of-shift` | Expressions that multiply a wage |
| `promotion.goal` | Contract points; `0` for no promotion |
| `promotion.deadline-hours` | How long from signing |
| `promotion.success`, `promotion.failure` | Actions |

See [Contracts](../features/contracts.md).

Related: [Contracts](../features/contracts.md)
