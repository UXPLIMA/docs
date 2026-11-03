---
title: Contract
order: 409
description: "One contract: sign it, follow it, or cancel it."
icon: handshake
---

`menus/contract.conf`: five rows. Each button is drawn only when it means something.

| Slot | Item | Shown | Click |
|---|---|---|---|
| 13 | The document: job, cost, hours, places, shift | Always | |
| 22 | Writable book: promotion progress and hours left | Signed | |
| 29 | Lime concrete, sign, with the cost | Not signed | Signs and pays |
| 33 | Red concrete, cancel: "What you paid to sign it stays spent." | Signed | Cancels |
| 36 | Arrow, back | Always | The contracts |
| 44 | Barrier, close | Always | |

A `view` list decides what is drawn: `jobs:contract-signed` is true for a contract the viewer holds, and `!` reads it the
other way. Extra tokens: `%contract_goal%` and `%contract_progress%` (percent through the goal). Actions `jobs:sign` and
`jobs:cancel`.

Related: [Contracts](../features/contracts.md)
