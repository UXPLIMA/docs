---
title: Catalogues
order: 403
description: Every quest on offer, categories, prerequisites, cooldowns and manual claiming.
icon: library
---

`mode = catalogue` with `reset { type = never }`. A catalogue quest is not drawn and does not reset; the player starts
it with `/quests start <group> <quest>` or from the board.

```hocon
{
  id       = build-a-shelter
  category = building          # what the window sorts it under
  requires = [ gather-wood ]   # not offered until these are finished
  cooldown = 12h               # how long after payment it may be taken again
}
```

| Key | Meaning |
|---|---|
| `category` | What the window sorts it under |
| `requires` | Quest ids that must be finished first; until then starting it says which |
| `cooldown` | How long after payment it may be taken again; without one, once is all |

## The shipped catalogue

`groups/catalogue.conf`, claimed by hand:

| Quest | Category | Asks for |
|---|---|---|
| `gather-wood` | gathering | 64 oak, birch or spruce logs |
| `build-a-shelter` | building | 100 blocks placed, after `gather-wood` |
| `feed-the-village` | farming | 32 crops harvested, again every 12 hours |

Related: [Groups and boards](groups-and-boards.md)
