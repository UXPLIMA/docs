---
title: Your first network
order: 204
description: Two public lobbies and a staff lobby, with counts by ping.
icon: flag
---

Two public lobbies and a staff lobby that `/quicklobby` never picks. On every lobby, `lobbies.conf` holds:

```hocon
lobbies {
  hub-1 {
    fallback-name = "Hub One"
    proxy-name    = "hub1"
    host          = "10.0.0.11"
    port          = 25566
    maximum       = 120
    slot          = 12
    icon          = "GRASS_BLOCK"
    colour        = "0"
    permission    = ""
    weight        = 1.0
  }
  hub-2 {
    fallback-name = "Hub Two"
    proxy-name    = "hub2"
    host          = "10.0.0.12"
    port          = 25566
    maximum       = 120
    slot          = 13
    icon          = "GRASS_BLOCK"
    colour        = "1"
    permission    = ""
    weight        = 1.0
  }
  staff {
    fallback-name = "Staff"
    proxy-name    = "staff"
    host          = "10.0.0.20"
    port          = 25566
    maximum       = 20
    slot          = 14
    icon          = "COMMAND_BLOCK"
    colour        = "good"
    permission    = "server.staff"
    weight        = 0.0
  }
}
```

1. On hub-1, set `this-lobby = "hub-1"` in `config.conf`; on hub-2, `"hub-2"`; on the staff lobby, `"staff"`.
2. Set `status.source = "ping"` so a crashed lobby shows offline rather than empty.
3. Name each lobby in every language under `lobby.<id>.name` in the messages files, or keep the `fallback-name`.
4. `/uxmmultilobby reload` on each.
5. Join hub-1 as a player: the compass in slot 4 opens the window, hub-1 glows as "You are here", the staff lobby is
   locked. `/quicklobby` sends you to hub-2.

To leave the staff lobby out of the window for players without the node, set `menu.hide-locked = true`.

Related: [lobbies.conf](../config/lobbies-conf.md), [Live counts](../features/live-counts.md)
