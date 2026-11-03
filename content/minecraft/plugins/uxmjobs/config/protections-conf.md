---
title: protections.conf
order: 502
description: Every key of protections.conf with its default.
icon: shield
---

```hocon
protections {
  spawner-fraction         = 0.1
  artificial-fraction      = 0.1
  hopper-fed-fraction      = 0.2
  hand-fed-bonus           = 1.25
  pay-in-creative          = false
  pay-for-fertilised       = false
  damage-share             = 0.5
  generator-materials      = ["stone", "cobblestone", "basalt", "obsidian"]
  forbidden-vehicles       = ["minecart", "boat", "chest_boat"]
  artificial-spawn-reasons = ["spawner", "spawner_egg", "dispense_egg", "trial_spawner", "built_snowman", "built_iron_golem", "slime_split"]
  boss-mobs                = ["ender_dragon", "wither", "warden", "elder_guardian"]
  disabled-worlds          = []
}
```

What each key does is on [Protections](../features/protections.md#grades). A block an earlier release kept in
`config.conf` moves here on the first start.

Related: [Protections](../features/protections.md)
