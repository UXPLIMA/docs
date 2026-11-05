---
title: trade.conf
order: 649
description: Trading between players in one window.
icon: arrow-left-right
---

Trading between players. Two players on this server open one window, each puts in what they offer from their own inventory, and the trade happens when both agree. What is offered stays in its owner's inventory until then, so nothing is lost when a trade is called off or the server stops.

The file is `plugins/uxmSkyblock/modules/trade.conf`. Every default below is the shipped value.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Whether players can trade. |
| `request-seconds` | `60` | How long a request to trade waits for an answer, in seconds. From 5 to 600. |
| `max-distance` | `-1` | How far apart two players may be to trade, in blocks. Below zero for anywhere on this server. |

## `window`

The trade window is menus/player-trade.conf. What follows draws the plain window a trade opens in while that file is missing or cannot hold a whole offer. Each is a material name.

| Key | Default | What it does |
|---|---|---|
| `divider` | `BLACK_STAINED_GLASS_PANE` | The column between the two offers. |
| `ready` | `LIME_CONCRETE` | The button a player presses when they agree, and the same button while they do not. |
| `waiting` | `RED_CONCRETE` |  |
| `other-ready` | `LIME_STAINED_GLASS_PANE` | What shows whether the other player agrees. |
| `other-waiting` | `RED_STAINED_GLASS_PANE` |  |
| `cancel` | `BARRIER` | The button that ends the trade. |
