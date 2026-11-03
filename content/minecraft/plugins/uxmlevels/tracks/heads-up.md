---
title: Heads-up
order: 406
description: The experience bar, the boss bar and the action bar, which track each shows, and the gain notice.
icon: gauge
---

Three surfaces can show a track's progress, set in each track file:

| Surface | Shows | Shipped track |
|---|---|---|
| `hud.experience-bar` | The vanilla experience bar: the track's level as the number, the progress into it as the bar | `global` |
| `hud.boss-bar` | A bar across the top: `hud.boss-bar`, `Mining | level 3, 26 per cent` | `mining` |
| `hud.action-bar` | A line over the hotbar: `hud.action-bar`, `Combat level 1 [9/150]` | `combat` |

Each surface shows one track. When two files ask for one, the first file in name order takes it, and the console
names the track that lost.

## The experience bar

<Callout type="warning" title="The experience bar track replaces vanilla experience">

Every refresh sets the player's level and progress to the track's, so vanilla experience is overwritten: experience
orbs, enchanting, anvils, and any plugin that pays or charges `exp` or `levels`, prestige with
`economy.currency = "exp"` included. Leave `hud.experience-bar` off on every track on a server that uses vanilla
experience.

</Callout>

## Timing

```hocon
hud {
  refresh-ms = 1000
  gain-notice-ms = 3000
}
```

| Key | Default | What it does |
|---|---|---|
| `refresh-ms` | `1000` | How often the surfaces are drawn again |
| `gain-notice-ms` | `3000` | How long a gain such as `+12 Mining` holds the action bar before the track line takes it back |

The gain notice shows for events with `action-bar = true`. A level up also sends `levels.level-up` in chat.

Related: [Tracks](tracks.md), [Messages](../config/messages.md)
