---
title: Load balancing
order: 303
description: How /quicklobby picks a lobby, what weight does, and ties.
icon: scale
---

`/quicklobby` sends the player to the lobby with the lowest load among the ones they may enter, leaving out the lobby
they are on.

```
load = players / (capacity * weight)
```

The capacity is the maximum the lobby reports, or the configured `maximum` when it reports none. A tie goes to the lobby
with fewer players, then to the lower id. Lobbies that are full, offline or unknown are left out. When no lobby is open,
the player gets the `nothing-available` outcome.

| `weight` | Effect |
|---|---|
| `1.0` | An even share |
| above 1 | Takes more players: a lobby on a bigger machine |
| below 1 | Takes fewer |
| `0` | Never picked; still in the window |

The window's `lobby:best` action does the same; the shipped window leaves its button out, commented in the file.

Related: [lobbies.conf](../config/lobbies-conf.md), [Menu file](../config/menu-files.md)
