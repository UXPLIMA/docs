---
title: currencies.conf
order: 509
description: The five currency types and their keys.
icon: coins
---

| Key | Meaning |
|---|---|
| `type` | `item`, `vault`, `experience`, `placeholder` or `treasury` |
| `order` | The lowest is the default currency |
| `enabled` | Turn it off without deleting it |
| `material` | `item` only |
| `balance`, `give`, `take` | `placeholder` only, with `{player}` and `{amount}` |
| `treasury-currency`, `wait-millis` | `treasury` only |

The shipped five are in [Currencies](../features/currencies.md).

Related: [Currencies](../features/currencies.md)
