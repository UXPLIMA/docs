---
title: Contracts
order: 408
description: The contracts this server offers, and the ones the viewer signed.
icon: scroll
---

`/contracts menu` opens `menus/contracts.conf`: five rows.

| Slot | Item | Click |
|---|---|---|
| 10 to 16, 19 to 25 | Each contract on offer: job, cost, hours, and whether the shift is open now | Opens [the contract](contract.md) |
| 28 to 34 | Each contract the viewer signed: hours left and points | Opens it |
| 44 | Barrier, close | |

| Token | Value |
|---|---|
| `%contract%`, `%contract_name%`, `%contract_description%`, `%contract_icon%` | The contract |
| `%contract_job%`, `%contract_cost%`, `%contract_hours%` | Its job, cost and length |
| `%contract_slots%`, `%contract_slots_state%` | Its places, and `limited` or `unlimited` |
| `%contract_state%` | `signed`, `offered` or `none` |
| `%contract_signed%` | `true` when the viewer holds it |
| `%contract_shift%` | `open` or `closed` right now |
| `%contract_points%`, `%contract_hours_left%` | For a signed one |

List sources `jobs:contracts` and `jobs:signed`; action `jobs:open-contract`. The shift line reads
`menu.contracts.offered.shift-open` or `shift-closed`.

Related: [Contract](contract.md), [Contracts](../features/contracts.md)
