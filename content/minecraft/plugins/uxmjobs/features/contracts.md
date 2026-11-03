---
title: Contracts
order: 408
description: "Documents a player pays for: working hours, in-shift and out-of-shift pay, promotions and cancelling."
icon: scroll
---

A contract is a document a player signs for a job, not a task. Signing costs money, so the player pays for the right to
be paid more and the money leaves the economy on the way in. Inside its working hours the wage carries `in-shift`;
outside them `out-of-shift`, a penalty. A job that pays more between 19:00 and 22:00 decides when a server is busy.

```hocon
name        = "@content.contracts.evening-shift.name"
description = "@content.contracts.evening-shift.description"
icon        = "clock"
job         = "miner"
cost        = "5000"
currency    = ""
duration-hours = 168
slots       = 0
requirements = []
cancel-cooldown-minutes = 60
working-hours {
  monday = ["19:00-22:00"]
  friday = ["19:00-23:00"]
}
in-shift     = "1.5"
out-of-shift = "0.8"
promotion {
  goal           = 500
  deadline-hours = 72
  success = ["[message] @contract.evening-shift.done", "[give-money] points 500"]
  failure = ["[message] @contract.evening-shift.late"]
}
```

| Key | Meaning |
|---|---|
| `job` | The job it is signed for |
| `cost`, `currency` | What signing costs, and in what; empty is the order of `economy.conf` |
| `duration-hours` | How long it runs before it ends on its own |
| `slots` | How many players may hold it at once; `0` for no limit |
| `requirements` | What a player must pass to sign |
| `cancel-cooldown-minutes` | How long after signing before it may be cancelled |
| `working-hours` | A day, and one or more windows written `19:00-22:00` |
| `in-shift`, `out-of-shift` | What a wage is multiplied by inside and outside those hours |
| `promotion` | A `goal` in contract points, a `deadline-hours`, and the `success` and `failure` actions |

## Contract points

Contract points are the job experience earned under the contract. They are a third balance, apart from job points, so a
contract never competes with the shop. A promotion succeeds when the goal is reached. Its failure is answered at the
first piece of work after the deadline.

## Signing and cancelling

`/contracts sign <contract>` or the sign button in the [contract window](../menus/contract.md); a dialog asks first when
dialogs are on. `/contracts cancel <contract>` walks away after the cooldown; what was paid stays spent.
`/contracts shift` says whether the hours are open now.

The shipped contracts: `evening-shift` (miner, 5000, a week, 1.5 inside 19:00 to 22:00 on weekdays and to 23:00 on
Friday, 0.8 outside, goal 500 in 72 hours for 500 points) and `weekend-crew` (woodcutter, 8000, two weeks, 10 places, 2.0
all weekend, 0.5 outside, goal 1200 in a week for 1500 points and a broadcast).

Related: [Contract windows](../menus/contracts.md), [Contract files](../config/contract-files.md)
