---
title: Zones
order: 311
description: Work zones drawn with a wand, their multipliers and requirements, and blocks that grow back.
icon: map
---

A zone is a box of the world that scales what work inside it is worth. It is a bonus layer and never a gate: nothing
pays less merely for being outside one. A multiplier of zero is a no pay zone, which is how a spawn or an arena is
marked without touching the world list.

## Drawing one

1. `/uxmjobs zone wand` gives the wand (`zones.wand-material`, a golden hoe), recognised by a tag on the item.
2. Left click marks the first corner, right click the other; neither breaks the block. A wire frame only you see is
   drawn around the box.
3. `/uxmjobs zone define <zone>` writes `zones/<zone>.conf`, or `/uxmjobs zone name` asks for the name in a screen.

## The file

```hocon
name        = "Deep mine"
description = ""
icon        = "beacon"
box { world = "world", min-x = 0, min-y = -64, min-z = 0, max-x = 50, max-y = 0, max-z = 50 }
multipliers { money = 1.5, experience = 1.2 }
requirements = []
renewable {
  blocks        = ["diamond_ore", "iron_ore"]
  fallback      = "stone"
  reset-seconds = 60
  drops         = true
}
```

| Key | Meaning |
|---|---|
| `multipliers` | What work inside is worth per currency; one where none is written |
| `requirements` | Condition lines; a player who fails them gets no bonus |
| `renewable.blocks` | Blocks the zone puts back after they are mined; empty owes nothing |
| `renewable.fallback` | What stands in the hole until then |
| `renewable.reset-seconds` | How long it stands (60) |
| `renewable.drops` | Whether the break drops anything |

The file is written whole each time the zone is defined again, so a comment you add does not survive; any value you edit
does, after a reload.

## Regeneration

Every `zones.regeneration-interval-seconds` (30) at most `regeneration-batch` (200) blocks that are due are put back.
`/uxmjobs zone regenerate` does it now. Deleting a zone puts back every block it still owed.

## The frame

`zones.frame`: `enabled`, `refresh-seconds` (1), `step` (1 block), `maximum-points` (800; the step widens past it),
`colour` (`#4ecca3`) and `size` (1). It is a packet per viewer: nothing is spawned or written.

`%uxmjobs_zone%` names the zone a player stands in. `zones.enabled = false` turns zones off.

Related: [Admin commands](../commands/admin.md), [Zone files](../config/zone-files.md)
