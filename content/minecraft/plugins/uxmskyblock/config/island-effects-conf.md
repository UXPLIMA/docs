---
title: island-effects.conf
order: 628
description: The potion effects upgrades put on players on the island.
icon: flask-conical
---

The file is `plugins/uxmSkyblock/modules/island-effects.conf`. Every default below is the shipped value.

## `island-effects`

The potion effects an island's upgrades put on the players standing on it.

Every upgrade named below is an effect upgrade: each property of its tiers in modules/upgrades.conf is a potion effect and its level, so "haste = 2" is Haste II. An island on that tier puts them on the players standing on it, and a player who leaves keeps them only until they wear off.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` |  |
| `every-seconds` | `3` | How often the effects are put on again, in seconds. Each lasts a little longer than this, so it never flickers, and wears off about this long after a player leaves. |
| `visitors` | `false` | Whether visitors wear them too, or only the island's members. |
| `upgrades` | `island_effects` | The upgrades whose tiers name effects. |
