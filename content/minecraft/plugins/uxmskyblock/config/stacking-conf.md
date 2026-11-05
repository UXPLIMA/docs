---
title: stacking.conf
order: 646
description: Stacked blocks and how they count.
icon: layers
---

The file is `plugins/uxmSkyblock/modules/stacking.conf`. Every default below is the shipped value.

## `stacking`

Block stacking

A block of a kind listed here, placed on a block of the same kind, joins it: the block stays one block in the world and stands for one more. Each block in a stack counts toward the island's worth and level, a line above it shows how many it stands for, and breaking it gives back one at a time. A player who sneaks places a block the usual way.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` |  |
| `blocks` | a list of 10 | The blocks that stack, by their names |
| `most` | `100000` | The most blocks one stack stands for |
