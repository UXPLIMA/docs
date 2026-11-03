---
title: currencies.conf
order: 605
description: The currencies a tag can be bought with, their types and how an amount is written.
icon: coins
---

A tag's `price` names one of these by its id. Nothing is converted between them. Turn a currency off with
`enabled = false` rather than deleting it, so the prices that name it still mean something. A currency whose plugin is
not installed is not ready, and a tag priced in it cannot be bought until it is.

| Currency | Type | Order | Enabled | Writes |
|---|---|---|---|---|
| `coins` | `vault` | 10 | yes | `@currency.coins`: `{amount} coins`, 2 decimals, trimmed, `,` thousands, `.` point |
| `points` | `playerpoints` | 20 | no | `@currency.points`, no decimals |
| `experience` | `experience`, unit `levels` | 30 | yes | `@currency.levels`, no decimals |

Types: `vault`, `vaultunlocked`, `playerpoints`, `ecobits`, `treasury`, `item`, `experience`, `bridge` and
`placeholder`. Each takes the same keys it takes in every UXPLIMA plugin.

`display.format` writes an amount, with `{amount}` the number; `format-one` is used for exactly one. A format may name
a line of the messages files (`@currency.coins`), so each player reads it in their own language.

Related: [Tags](../features/tags.md#selling-a-tag)
