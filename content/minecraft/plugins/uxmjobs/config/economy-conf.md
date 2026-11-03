---
title: economy.conf
order: 603
description: The cost order, batching, the Treasury wait, the placeholder backend and bridges.
icon: landmark
---

| Key | Default | Meaning |
|---|---|---|
| `currency` | `["vault", "exp"]` | The order a cost is taken in; `[]` makes every cost free |
| `batch-seconds` | 5 | How long wages gather before they are paid |
| `treasury-wait-ms` | 250 | How long one Treasury call may take |
| `placeholder.<key>` | none | `balance`, `take`, `thousands` |
| `bridges.<name>` | none | An economy described by its class and methods |

Every key is on [Economy](../features/economy.md).

Related: [Economy](../features/economy.md)
