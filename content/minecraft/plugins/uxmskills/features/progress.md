---
title: Progress and rewards
order: 308
description: Notifications, level up and capstone actions, specialisation, death and the activator block.
icon: bell
---

## Notifications

| Key in `notifications` | Default |
|---|---|
| `pooling-window-ms` | 2000: awards pooled and shown as one figure |
| `action-bar` | true |
| `chat` | false |
| `boss-bar` | false; its colour is the skill's colour role |
| `boss-bar-linger-ms` | 4000 |

`progress-bar` draws every bar a placeholder asks for: `width` 20, `filled` `|`, `empty` `.`.

## Level up and capstone

`level-up.actions` (a message and the level up sound) run on every level; `capstone.actions` when any skill is
finished, beside the skill's own `capstone`. Lines take `@key`.

## Specialisation

```hocon
specialisation {
  default = 0
  nodes { "uxmskills.specialise.three" = 3 }
}
```

Above zero, a player earns only in the skills they have chosen (`/skills choose`, `/skills forget`). The number comes
off a node you name; `default` is for a player with none. Zero, which ships, is no limit.

## Death

```hocon
death { enabled = false, whole-level = false, keep-ratio = 0.5, skills = [] }
```

`whole-level = false` keeps `keep-ratio` of the experience put into the current level; `true` drops to the bottom of
the level below. `skills` empty touches every one.

## The activator

```hocon
activator { enabled = false, material = "ENCHANTING_TABLE", name = "", vertical-faces = true }
```

A block a player right clicks to open the whole plugin. `name` limits it to blocks whose display name holds that text.

Related: [config.conf](../config/config-conf.md)
