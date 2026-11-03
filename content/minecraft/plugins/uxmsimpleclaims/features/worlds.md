---
title: Worlds
order: 401
description: World modes, protected heights and the rules of the wilderness.
icon: globe
---

```hocon
worlds {
  default-mode = "normal"
  modes {
    world_nether  = "normal"
    world_the_end = "normal"
  }
  heights {
    # world { min = -64, max = 320 }
  }
}
```

| Mode | Means |
|---|---|
| `normal` | Claims are allowed; outside them the wilderness rules apply |
| `claims-required` | Claims are allowed; nobody builds outside one |
| `protected` | Nobody claims and nobody changes anything |
| `disabled` | This plugin leaves the world alone |

`heights` sets the lowest and highest block a claim protects, per world; a world not listed is protected from bottom to
top.

## The wilderness

```hocon
flags {
  wilderness {
    default { }
    # world_nether { pvp = true }
  }
}
```

The rules of the land outside claims, per world, with `default` for the rest. A flag not written is allowed.

Related: [config.conf](../config/config-conf.md#worlds)
