---
title: Set pads
order: 402
description: A pad on one block that throws one fixed way, how to set, list and remove it, and the pads.conf file.
icon: map-pin
---

A matched pad throws where the player looks. A set pad throws where the operator looked when they set it, whoever
steps on it: the pad that sends players down a path or over a wall.

| Command | Does |
|---|---|
| `/uxmjumppads set <type>` | Makes the block you stand on a pad of that type, throwing the way you face |
| `/uxmjumppads remove` | Takes the set pad at your feet or under you away |
| `/uxmjumppads list` | Every set pad |

All three need `uxmjumppads.admin.edit`. See [Operator commands](../commands/admin.md).

## Rules

| Rule | Meaning |
|---|---|
| Any block | A set pad need not be the type's `block` on its `under`: a plain stone block can be one |
| The type gives the speed | `forward`, `upward`, `permission` and `effects` come from the type, read at each step, so a reload changes every set pad of that type |
| It wins | On a block that also matches a type, the set pad throws |
| One per block | A second `set` on the same block replaces the first |
| Its type must exist | A set pad whose type is gone from `config.conf` throws nobody, and `doctor` warns about it |

## pads.conf

Set pads are kept in `pads.conf`, written whole each time a pad is set or removed:

```hocon
pads = [
  { spot = "world 12 64 -3", type = "classic", yaw = 180.0 }
]
```

| Key | Meaning |
|---|---|
| `spot` | The world and the block, as `world x y z` |
| `type` | The pad type |
| `yaw` | The direction of the throw, in degrees: 0 south, 90 west, 180 north, -90 east |

The file is data, read at start. Edit it with the server stopped; a reload does not read it. An entry that cannot be
read is left out with a warning.

Related: [Pad types](types.md), [Operator commands](../commands/admin.md)
