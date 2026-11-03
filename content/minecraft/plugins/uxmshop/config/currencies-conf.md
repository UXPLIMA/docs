---
title: currencies.conf
order: 602
description: The keys of a currency entry and its display block.
icon: coins
---

| Key | Meaning |
|---|---|
| `type` | One of the nine types |
| `order` | The lowest is the first tab and the default |
| `enabled` | Turn it off without deleting it |
| `pays-offline` | Whether an owner who is away can be paid; true by default |
| `material` | `item` |
| `unit` | `experience`: `levels` or `points` |
| `currency-name` | `ecobits`, `treasury` |
| `wait-for` | `treasury` |
| `placeholder`, `take`, `give`, `thousands`, `whole` | `placeholder` |
| `plugin`, `class`, `access`, `accessor`, `balance`, `take`, `give`, `argument`, `answer`, `whole`, `currencies`, `currency-name`, `currency-class`, `currency-lookup`, `take-negates`, `naming` | `bridge` |
| `display` | `format`, `format-one`, `decimals`, `trim`, `thousands`, `point`, `short`, `short-suffixes`, and `gui`, `chat`, `hologram` |

The shipped currencies and what each type means are in [Currencies](../features/currencies.md). The file ends with
commented examples of a placeholder currency and a bridge.

Related: [Currencies](../features/currencies.md)
