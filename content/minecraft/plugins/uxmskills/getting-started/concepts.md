---
title: Concepts
order: 103
description: Skills, sources, curves, stats, traits, abilities, resources and the rate.
icon: lightbulb
---

| Word | Means |
|---|---|
| Skill | One file under `skills/`; its file name is its id. Experience and a level |
| Source | An entry of the skill's price list under `sources/`: a kind of work and what it pays |
| Curve | What the step from one level to the next costs |
| Stat | Strength, toughness and the rest: points a level pays, turned into traits |
| Trait | A concrete effect of a stat: melee damage, damage taken, fishing luck |
| Ability | A behaviour a skill opens at a level, stronger with each ability level |
| Resource | Mana or stamina an ability spends; both ship off |
| Rate | The anti-farm multiplier on what a player earns right now |
| Capstone | What finishing a skill pays, once |
| Board | A ranking of one skill, the total or the average, over a period |

Every number can be a formula. A name, description or badge is a line of the messages files
(`@content.skills.mining.display.name`), a block per language (`{ en = "Mining", tr = "Madencilik" }`), or one text
for everybody; the shipped files use the first, so a new language is one more messages file.

Related: [Skills](../features/skills.md)
