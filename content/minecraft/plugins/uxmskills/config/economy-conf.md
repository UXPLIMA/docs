---
title: economy.conf
order: 503
description: Money backends for requirements, costs and rewards.
icon: coins
---

uxmSkills pays nobody; a source's `money` is a figure it publishes. This file is the other direction: a requirement
that costs money, an ability that charges for a swing, a reward that takes something.

| Backend | Is |
|---|---|
| `exp` | The player's experience, in points; needs no plugin |
| `levels` | The same, in levels; needs no plugin |
| `vault` | The original Vault |
| `vault-unlocked` | VaultUnlocked |
| `playerpoints` | PlayerPoints, whole points |
| `ecobits:<currency>` | EcoBits, which always wants a currency |
| `treasury` | Treasury; a currency is optional; `treasury-wait-ms` 250 bounds a call |
| `placeholder:<currency>` | A balance a placeholder answers, under `placeholder` |
| A bridge | Any economy you describe, under `bridges` |

```
money exp >= 250                          a requirement, in a skill file
money levels >= 5                         the same, in levels
[take-money] exp 250                      a reward that takes
cost { currency = "exp", money = 25 }     an ability's own charge
```

Every shipped example names `exp` or `levels`, so the box plays on the day it arrives. A file naming a backend this
server does not run is refused at load with the file and key, and skipped.

Related: [Skills](../features/skills.md)
