---
title: titles.conf
order: 509
description: The title ladder, per job and for every job.
icon: award
---

```hocon
titles {
  default { "1" = "...", "25" = "...", "50" = "...", "75" = "...", "100" = "..." }
  miner   { "100" = "..." }
}
```

A player wears the highest entry at or below their level; a job's own block is asked before `default`. A title is
content, not a message: renaming the level fifty Miner is editing your server. The shipped words are under
`content.titles.titles` in each messages file.

Related: [Rewards and titles](../features/rewards-and-titles.md#titles)
