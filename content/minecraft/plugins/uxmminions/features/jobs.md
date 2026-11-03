---
title: The ten jobs
order: 402
description: What each job does with its targets, range and level.
icon: hammer
---

| Job | What it does |
|---|---|
| `miner` | Breaks the block it faces, and the blocks beside it as its range grows |
| `lumberjack` | Chops the whole tree it faces and replants it; the range caps the blocks broken per action |
| `farmer` | Harvests the mature crops in range and replants them |
| `fisher` | Fishes from the water in range, drawing from its `loot` by weight; with no water in range it stops and says why |
| `slayer` | Kills the hostile mobs in range and takes their drops |
| `collector` | Sweeps dropped items in range into its destination |
| `feeder` | Feeds fuel to neighbouring minions from its own stock; never a feeder, never itself |
| `seller` | Sells the contents of its destination on its own interval, through the price sources |
| `spawner` | Drives the mob spawner it faces, so it keeps producing while its owner is present |
| `healer` | Runs its `effects` lines on its owner and trusted players in range |

## The fisher's sea

```hocon
loot = [
  { item = COD,            weight = 45, min = 1, max = 2 }
  { item = NAUTILUS_SHELL, weight = 1,  min = 1, max = 1 }
]
```

A line of weight one beside one of weight nine is drawn one time in ten; nothing has to add up to a hundred. The shipped
sea: cod 45, salmon 25, kelp 10, ink sac 6, tropical fish 5, pufferfish 4, prismarine shard 3, nautilus shell 1,
enchanted book 1.

## The healer's lines

```hocon
effects = [
  "[effect] regeneration 6 1"
  "[effect] saturation 2 1 true"
  "[particle] HEART 6 0.6"
  "[sound] minecraft:block.amethyst_block.chime 0.4 1.8"
]
```

There is no heal amount in the plugin. Every verb works: `[effect] <effect> <seconds> [level] [hidden]`, `[message]`,
`[actionbar]`, `[title]`, `[subtitle]`, `[bossbar]`, `[sound]`, `[particle]`, `[console]`, `[player]`, `[take-money]`,
`[take-item]`, each with `{delay=N, repeat=N, every=N, chance=0.5, if=...}`. It reaches the owner and their trusted
players and nobody else; a tick that reaches nobody costs no fuel.

## Jobs of other plugins

`job = "their-plugin:harvester"`, quoted because a colon separates keys in the file format. The namespace keeps it from
being read as a typo of one of the ten.

Related: [Types and levels](types-and-levels.md), [The shipped types](catalogue.md)
