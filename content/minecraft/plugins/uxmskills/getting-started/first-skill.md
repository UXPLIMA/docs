---
title: Your first skill
order: 104
description: A twelfth skill from a copy, with its own sources and an ability.
icon: footprints
---

1. Copy `skills/mining.conf` to `skills/quarrying.conf` and `sources/mining.conf` to `sources/quarrying.conf`.
2. In `skills/quarrying.conf`, write `sources = "quarrying"`, a name and a slot:

```hocon
display {
  name  { en = "Quarrying", tr = "Taş ocağı" }
  icon  = "STONE_BRICKS"
  slot  = 23
}
abilities = ["double-drop"]
```

3. In `sources/quarrying.conf`, keep the entries for stone and change what each pays.
4. Run `/uxmskills reload`. `/skills` shows the twelfth skill; nothing lists the skills, so nothing else is told.
5. Grant `uxmskills.skill.quarrying`: only the shipped eleven are held by everybody.
6. Mine stone and watch the action bar. `/skills sources quarrying` shows the price list in game; after a restart
   `/quarrying` opens it too.

Related: [Skills](../features/skills.md), [Sources](../features/sources.md)
