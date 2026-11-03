---
title: The shipped jobs
order: 418
description: The eight example jobs, with their curves, costs, action types and rewards.
icon: book-open
---

Every shipped job caps at level 100, has no seat limit, no join fee and no requirements, and lowers its leave cost for
`uxmjobs.leave.discount`. The two highest rewards of every job, marked "claim", are written `automatic = false` and
are not given in this build; see [Rewards and titles](rewards-and-titles.md).

| Job | Icon | Curve | Leave cost (discount) | Action types | Rewards at |
|---|---|---|---|---|---|
| Builder | bricks | linear 130, step 55 | 500 + 25/level (250 + 10) | `place` (34 blocks) | 10, 25, 50 claim, 100 claim |
| Enchanter | enchanting table | geometric 160, 1.085 | 700 + 35/level (350 + 15) | `enchant` (38), `brew` (4), `craft` (9), `trade` (4) | 10, 25, 50 claim, 100 claim |
| Farmer | iron hoe | linear 110, step 45 | 400 + 20/level (200 + 8) | `mine` (15 crops), `block_drop` (7), `breed` (10), `milk` (3), `shear` (16 wools), `dye` (4), `receive` (bone meal) | 10, 25, 50 claim, 100 claim |
| Fisherman | fishing rod | geometric 130, 1.07 | 400 + 20/level (200 + 8) | `fish_item` (20), `fish_mob` (8), `bucket_fill` (6) | 10, 30, 50 claim, 100 claim |
| Hunter | iron sword | geometric 150, 1.08 | 600 + 30/level (300 + 12) | `kill` (34), `mob_drop` (16), `kill_job` (hunter, miner, smith) | 10, 20, 50 claim, 100 claim |
| Miner | iron pickaxe | geometric 140, 1.075 | 500 + 25/level (250 + 10) | `mine` (29), `block_drop` (9) | 10, 25, 50 claim, 75, 100 claim |
| Smith | anvil | geometric 140, 1.075 | 500 + 25/level (250 + 10) | `smelt` (17), `campfire` (6), `craft` (15), `repair` (10), `smith` (9), `grind` (4) | 10, 25, 50 claim, 100 claim |
| Woodcutter | iron axe | geometric 120, 1.07 | 400 + 20/level (200 + 8) | `mine` (16 logs and leaves), `block_drop` (4), `strip` (10), `wax` (4), `scrape` (4) | 10, 25, 50 claim, 100 claim |

## Miner

"Ore, stone and deepslate that the world made. Not what you put there." Stone 0.35 + 0.01/level, deepslate 0.45 +
0.012/level, the ores from 2.0 (coal) to 5.5 (deepslate gold), each rising with the level. Emerald and diamond pay 18 to
25 plus 0.25 to 0.3 per level, a point or half a point, and stop rising at level 70. Ancient debris pays from level 40,
60 + 0.5/level and 3 points, until level 80. Gravel stops paying at level 30. The drops are priced apart: diamond 8,
emerald 6, raw gold 1.8, raw iron 1.2. Bounds: money -50 to 200, experience 0 to 150, points 0 to 20. Rewards: a message
at 10, the node `uxmjobs.perk.miner.deep` at 25, 250 points at 50, a broadcast and `uxmjobs.perk.miner.debris` at 75,
2500 points and a title at 100. `explore` is written in the file, commented out.

## The others

- **Builder**: "Paid for what you put down, once per place, and never for the same spot twice."
- **Enchanter**: "The table, the stand and the librarian. Once per item, never twice."
- **Farmer**: "Crops grown out, animals raised, and wool priced by its colour."
- **Fisherman**: "What the water gives up, priced by what came out of it."
- **Hunter**: "What fights back, and what drops when it stops." Kills a hunter, a miner or a smith for a bounty.
- **Smith**: "The furnace, the anvil, the smithing table and the grindstone."
- **Woodcutter**: "Trees the world grew, and the small trades an axe already does."

Run `/uxmjobs preview <job>` for every row's numbers.

Related: [Jobs and prices](jobs-and-prices.md), [Action types](action-types.md)
