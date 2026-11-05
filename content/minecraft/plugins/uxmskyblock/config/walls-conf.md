---
title: walls.conf
order: 654
description: The walls between islands.
icon: brick-wall
---

Walls.

A wall marks where ground ends. The border a client draws is one square, and a ChunkBlock island's open chunks grow into any shape, so the plugin stands its own wall on every side where an open chunk meets a closed one. Members and visitors see the same wall.

A wall is drawn with display entities that are never saved: each chunk stands its walls when it loads, and a chunk that opens or closes redraws its own and its neighbours'. A Bedrock client draws no display entities, so a Bedrock player is shown the edge as a line of dust instead, below.

The file is `plugins/uxmSkyblock/modules/walls.conf`. Every default below is the shipped value.

## `walls.styles`

The styles a wall can be drawn in. Add as many as you like.

kind "colour" is a flat panel of one colour. colour is #RRGGBB, and opacity is how much of what stands behind it the panel hides, from 0 to 100. kind "block" is a thin slab cut from a block: glass, wool, anything with a block id, as minecraft:pink_stained_glass.

## `walls.styles.mist`

| Key | Default | What it does |
|---|---|---|
| `kind` | `colour` |  |
| `colour` | `#8A93A1` |  |
| `opacity` | `22` |  |
| `walls.styles.glass.kind` | `block` |  |
| `walls.styles.glass.block` | `minecraft:light_gray_stained_glass` |  |
| `walls.chunkblock` | `mist` | The style around a ChunkBlock island's open chunks. "" stands no wall. |
| `walls.island` | `border` | How an island's edge is drawn until its members choose otherwise in the border window: "border" is the square border a client draws, in the colour each player picks; a style's id stands a wall of that style; "none" draws nothing. A ChunkBlock island is always edged by the wall around its open chunks instead. |
| `walls.bottom` | `-64` | How high a wall reaches. A client draws a wall entity only while it is near enough, so a wall is cut into pieces slice blocks tall, each drawn by the player who stands near it. |
| `walls.top` | `320` |  |
| `walls.slice` | `64` |  |

## `walls.bedrock`

The edge as a Bedrock player sees it: a line of dust along every wall near them, shown to them alone, once a second.

```text
colour    the dust's colour, #RRGGBB
spacing   how far apart two specks stand along the edge, in blocks, 0.25 or more
reach     how far around the player the edge is drawn, 1 to 48 blocks
```

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` |  |
| `colour` | `#8A93A1` |  |
| `spacing` | `1.0` |  |
| `reach` | `12` |  |
