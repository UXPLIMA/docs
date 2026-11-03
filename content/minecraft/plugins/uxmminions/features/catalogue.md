---
title: The shipped types
order: 415
description: The ten example types, their intervals, targets, ladders and upgrade costs.
icon: book-open
---

Every shipped type has ten levels. Storage is three slots per level (3 to 30), the range grows from 1 to 5 every two
levels (the healer's from 4 to 14), the sale multiplier rises from 1.0 to 1.45 and the tax falls from 0.1 to 0.01. From
level 4 a level also asks for 250 more actions done per level (250 at 4, 1750 at 10).

| Type | Job | Icon | Interval, level 1 to 10 | Targets | Level 2 costs | Level 10 costs |
|---|---|---|---|---|---|---|
| Collector | collector | hopper | 100 to 55 | everything | 3600, 64 hoppers | 90000, 576 hoppers |
| Farmer | farmer | iron hoe | 60 to 33 | wheat, carrots, potatoes, beetroots, nether wart | 4000, 64 wheat | 100000, 576 wheat |
| Feeder | feeder | bucket | 300 to 165 | everything | 6000, 64 coal | 150000, 576 coal |
| Fisher | fisher | fishing rod | 60 to 33 | the sea | 5200, 64 cod | 130000, 576 cod |
| Healer | healer | golden apple | 200 to 110 | owner and trusted | 6000, 64 coal | 150000, 576 coal |
| Lumberjack | lumberjack | iron axe | 200 to 110 | eleven logs and stems | 5600, 64 oak logs | 140000, 576 oak logs |
| Miner | miner | iron pickaxe | 50 to 28 | everything but a blacklist | 4800, 64 cobblestone | 120000, 576 cobblestone |
| Seller | seller | emerald | 300 to 165 | everything | 8000, 64 gold ingots | 200000, 576 gold ingots |
| Slayer | slayer | iron sword | 140 to 77 | eighteen hostile mobs | 6400, 64 rotten flesh | 160000, 576 rotten flesh |
| Spawner | spawner | spawner | 200 to 110 | spawner | 9600, 64 blaze rods | 240000, 576 blaze rods |

Costs are in `economy.currency`, experience points as shipped. The money cost grows with the square of the level: a
type's base times the level times itself (collector 900, farmer 1000, miner 1200, fisher 1300, lumberjack 1400, feeder
and healer 1500, slayer 1600, seller 2000, spawner 2400). The item cost is 64 times one less than the level.

## Notes

- **Miner** smelts its output from level 7, and holds a wooden pickaxe at 1 up to netherite at 10. Its blacklist keeps
  it off bedrock, fluids, spawners, vaults, containers, furnaces, command and structure blocks, portals, beacons and
  conduits.
- **Farmer** never works podzol, coarse dirt, rooted dirt or mycelium; its hoe goes from wood to netherite.
- **Lumberjack** cuts oak, spruce, birch, jungle, acacia, dark oak, mangrove, cherry and pale oak logs, and crimson and
  warped stems; its axe goes from wood to netherite.
- **Slayer** kills zombies, skeletons, creepers, spiders, cave spiders, endermen, witches, slimes, husks, strays,
  drowned, pillagers, blazes, magma cubes, zombified piglins, piglins, wither skeletons and phantoms; its sword goes
  from wood to netherite.
- **Healer** gives regeneration for 6 seconds and a hidden saturation, with hearts and a chime.
- **Seller** holds gold, then an emerald at level 10.

Quotas: three spawners, five sellers and five feeders per scope, out of 25 in all.

Related: [Types and levels](types-and-levels.md), [The ten jobs](jobs.md)
