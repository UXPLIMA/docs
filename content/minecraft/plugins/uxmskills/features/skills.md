---
title: Skills
order: 401
description: The keys of a skill file, gates, loot, and the eleven shipped skills.
icon: zap
---

Everything about a skill is in its file under `skills/`. Delete the file and the skill is gone; copy it under another
name and a twelfth exists. Nothing lists the skills.

| Key | Meaning |
|---|---|
| `enabled` | `false` takes the skill out of every window, placeholder and board; stored experience stays |
| `display` | `name` and `description` (per language), `icon`, `slot`, `colour` (a theme role) |
| `maximum-level` | The cap; lowering it never demotes anybody |
| `curve` | What each level costs; left out, the shared `curve` of `config.conf`. See [Curves](curves.md) |
| `stat-points` | Points of each stat one level pays, each an expression over `level`; `level % 2` pays every other level |
| `abilities` | The ability files this skill opens; move a name to another skill file and the ability moves |
| `sources` | The file under `sources/` that says how it is earned |
| `earn-conditions` | uxmLib conditions a player must pass to earn at all, such as `"money exp >= 250"`; empty for anybody |
| `capstone` | What finishing the skill pays, once: `badge` and `actions` |
| `gates` | What the skill holds back until earned: an item gate is a level, a block gate is a condition list. Nothing is gated when shipped |
| `loot` | Drops on top of the experience: a pool rolls at most once per payout, only for the source lines it names, and a point of luck moves its chance. Every shipped pool is empty |

A player also needs `uxmskills.skill.<id>` to earn; the shipped eleven are held by everybody.

## The shipped skills

All cap at level 100 and take the shared curve except agility, which writes
`base * level * (1 + ln(level) / 3)` with base 130.

| Skill | Earned by | Sources | Stat points | Abilities | Capstone badge |
|---|---|---|---|---|---|
| `mining` | Stone, deepslate and ore broken with a pickaxe, weighted by the block | `block-break`, `discovery` | endurance 1, fortune every 2nd level | vein-miner, double-drop, tool-saving | Master of the Deep |
| `excavation` | Dirt, sand, gravel, clay, snow and soft ground dug with a shovel | `block-break` | endurance 1, fortune every 3rd | wide-dig, soil-yield | Master of the Spade |
| `woodcutting` | Logs felled, and the trees you grew from a sapling | `block-break`, `discovery` | endurance 1, fortune every 3rd | tree-feller, timber-yield | Master of the Grove |
| `farming` | Mature crops harvested, animals bred, and honey and wool taken | `block-break`, `block-collect`, `block-interact` | wisdom 1, fortune every 3rd | auto-replant, herd-master, harvest-yield | Master of the Field |
| `fishing` | What a rod pulls out of the water, weighted towards open water | `fishing` | fortune 1, wisdom every 3rd | treasure-hunter, quick-bite | Master of the Line |
| `combat` | Melee damage dealt to a hostile mob, and the kill that ends it | `entity-damage`, `entity-kill` | strength 1, toughness every 2nd | critical-strike, bleed, life-steal | Master of the Blade |
| `archery` | Damage dealt at range with a bow, a crossbow or a thrown trident | `entity-damage`, `entity-kill` | dexterity 1, strength every 3rd | pierce, arrow-retrieval, daze | Master of the Draw |
| `defence` | Damage taken and survived, and damage stopped with a shield | `damage-taken`, `damage-blocked` | toughness 1, endurance every 3rd | thick-skin, shield-wall, second-wind | Master of the Wall |
| `agility` | Distance covered sprinting, swimming, climbing and gliding | `movement-pulse` | dexterity 1, endurance every 2nd | swift, soft-landing, deep-lungs | Master of the Road |
| `alchemy` | Potions brewed on a stand, and effects landed on another player | `brewing`, `item-consume`, `potion-splash` | wisdom 1, fortune every 4th | potion-master, frugal-brewer | Master of the Flask |
| `enchanting` | Work done at an enchanting table, an anvil and a grindstone | `enchanting`, `anvil-use`, `grindstone-use`, `item-consume` | wisdom 1, dexterity every 4th | arcane-saving, anvil-saving | Master of the Word |

Related: [Skill files](../config/content-files.md#skills), [Sources](sources.md)
