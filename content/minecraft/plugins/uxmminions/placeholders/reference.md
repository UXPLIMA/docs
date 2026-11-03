---
title: Reference
order: 701
description: Every %uxmminions_...% placeholder, per player and per minion.
icon: list
---

Registered when PlaceholderAPI is installed. Nothing reads the database on the call: per player numbers are read every
`placeholders.refresh-ticks` (40), per minion numbers ride `render.menu-refresh-ticks`. A number whose first read has
not landed answers an empty string, never a zero.

## Per player

| Placeholder | Returns |
|---|---|
| `%uxmminions_placed%` | How many they have placed |
| `%uxmminions_limit%` | How many they may place, granted slots included |
| `%uxmminions_remaining%` | The difference |
| `%uxmminions_placed_<type>%` | How many of one type |
| `%uxmminions_limit_<type>%` | The quota of one type |
| `%uxmminions_earned%` | Everything the ledger has paid them |

## Per minion, by id

Written `%uxmminions_minion_<id>_<fact>%`, with the id from the panel.

| Fact | Returns |
|---|---|
| `type`, `level`, `name` | Its type, level, and the name its owner gave (empty when none) |
| `world`, `facing` | Where it stands and which way it faces |
| `working` | `true` or `false` |
| `why` | Why it is not working |
| `fuel` | Actions left in its fuel |
| `actions` | Actions it has performed |
| `stock`, `capacity` | Items held, and room |
| `destination` | `internal`, `sell` or `chest` |
| `earnings` | What it has paid its owner |

A minion is never addressed by its order in a list, which changes the moment one is picked up.

Related: [Limits](../features/limits.md)
