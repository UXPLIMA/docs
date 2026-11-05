---
title: warps.conf
order: 655
description: "Island warps: how many, categories and who may open them."
icon: map-pin
---

Island warps: places on an island its team opens to visitors, who reach them from /is warps.

The file is `plugins/uxmSkyblock/modules/warps.conf`. Every default below is the shipped value.

## `warps`

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | false turns island warps off. |
| `base-warp-limit` | `2` | How many warps an island may open before the warp slots upgrade adds more. |
| `search-radius` | `5` | How far, in blocks, around a warp a safe spot to land is looked for when the warp itself is blocked or over a drop. |

## `warps.sign`

A sign whose first line is the header and whose second is a name makes a warp of that name where it stands, as /is setwarp does, with the same role permission and the same limit. Breaking the sign deletes the warp.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` |  |
| `header` | `[islandwarp]` | Read in any case. Not [warp]: Essentials and its kind read that as a server warp sign, and would take the sign for theirs. |
