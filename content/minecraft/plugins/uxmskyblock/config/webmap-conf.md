---
title: webmap.conf
order: 656
description: Islands drawn on a web map.
icon: map
---

Web map.

What Dynmap, BlueMap and Pl3xMap show of the islands. A web map is one page every visitor reads, so write its words in the language your players read.

The file is `plugins/uxmSkyblock/modules/webmap.conf`. Every default below is the shipped value.

## `webmap`

| Key | Default | What it does |
|---|---|---|
| `layer` | `Islands` | The name of the layer the islands are drawn on. |

## `webmap.top`

The colours of an island's area. The ten highest ranked islands are drawn as top, the islands of an alliance as allied, and every other island as other. Opacity runs from 0, clear, to 1, solid. Weight is the width of the border line.

## `webmap.top.allied.other.words`

The words of a marker. name is the label of an island that has no name of its own, and `<island>` in it is the first eight letters of the island's id.

| Key | Default | What it does |
|---|---|---|
| `name` | `Island <island>` |  |
| `owner` | `Owner` |  |
| `level` | `Level` |  |
| `rank` | `Rank` |  |
| `unranked` | `Unranked` |  |
| `worth` | `Net worth` |  |
| `bank` | `Bank` |  |
