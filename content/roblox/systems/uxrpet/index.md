---
title: uxrPet
order: 33
description: Eggs, hatching, rarities, variants, following pets, breakables, merging and trading.
icon: paw-print
---

A complete pet simulator. Players buy eggs, hatch pets with weighted odds, equip a squad
that trails behind them, send that squad to smash coin piles, merge duplicates into rarer
variants and trade with each other.

Fifteen config files, one drop-in folder, and a server API for wiring it into the rest of
your game.

## Pages

- [Setup](setup.md)
- [Pets, rarities and variants](pets.md)
- [Eggs and hatching](eggs.md)
- [Following and power](following.md)
- [Breakables and zones](breakables.md)
- [Merging and the index](merging.md)
- [Trading](trading.md)
- [Currencies and gamepasses](monetization.md)
- [The server API](api.md)
- [Settings](settings.md)

## What it does

| | |
|---|---|
| Pets | A catalog of species with rarity, power, flight and a follow offset |
| Variants | Golden, Rainbow, Huge and combinations, each a multiplier and a model prefix |
| Eggs | Weighted pools, multi-open, coin or Robux prices, world platforms |
| Luck | Gamepass tiers that bias specific pool entries |
| Following | A trailing grid with hopping, flying, banking and separation |
| Power | Equipped pets combine into one boost value, summed or multiplied |
| Breakables | Coin piles your pets attack, with shared rewards and respawns |
| Merging | Five of one variant into one of the next |
| Index | A collection log with equip-slot milestones |
| Trading | Two-sided offers with a confirmation countdown |
| Announcements | Rare hatches broadcast to every server |
| API | Around thirty server functions and eight signals |

## The shape of it

Everything a designer touches is a Lua table in `Shared/Config`, plus models dropped into
`Storage`. Pet models, egg models and breakable models are found by name, so adding a pet is
a config block and a model with a matching name.

The world side is a folder in `Workspace` holding zones and spawned pets, plus egg platforms
found by tag.
