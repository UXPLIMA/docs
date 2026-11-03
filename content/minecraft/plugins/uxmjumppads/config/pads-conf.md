---
title: pads.conf
order: 502
description: The file of the pads set on single blocks.
icon: map-pin
---

`pads.conf` holds the pads set with `/uxmjumppads set`. The plugin writes it whole on every `set` and `remove`, and
reads it at start. It appears with the first `set`.

```hocon
pads = [
  { spot = "world 12 64 -3", type = "classic", yaw = 180.0 }
  { spot = "world -40 70 18", type = "bounce", yaw = 0.0 }
]
```

| Key | Meaning |
|---|---|
| `spot` | The world and the block, written `world x y z` |
| `type` | The pad type, a name under `pads` in `config.conf` |
| `yaw` | The direction of the throw, in degrees: 0 south, 90 west, 180 north, -90 or 270 east |

Edit it with the server stopped: a reload does not read it, and the next `set` or `remove` writes over what you
changed. An entry that cannot be read is left out with a warning. An entry whose type `config.conf` does not have
throws nobody, and `/uxmjumppads doctor` names it.

Related: [Set pads](../pads/set-pads.md), [config.conf](config-conf.md)
