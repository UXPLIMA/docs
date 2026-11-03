---
title: Cooldowns
order: 306
description: The time before a click works again, per item and by default.
icon: timer
---

| Where | Key | Default |
|---|---|---|
| The item | `cooldown` | Left out: the default below |
| `config.conf` | `items.default-cooldown` | `"500ms"` |

Units are `ms`, `s`, `m`, `h` and `d`, combined as `"1m30s"`. `"0s"` is no cooldown.

The default is not zero on purpose: without it, holding the right mouse button on a server selector sends the connect
message twenty times a second. A click within the cooldown answers `item.cooldown`: "Wait N seconds."

Related: [config.conf](../config/config-conf.md)
