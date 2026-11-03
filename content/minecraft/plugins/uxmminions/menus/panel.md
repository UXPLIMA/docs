---
title: Panel
order: 402
description: "What a click on a minion opens: status, storage, upgrade, fuel, destination, skin, trust, facing and more."
icon: panel-top
---

`menus/panel.conf`: five rows titled with the minion's name, redrawn every 20 ticks.

| Slot | Item | Click |
|---|---|---|
| 4 | Paper: level, status and fuel | |
| 12, 13, 14, 16 | Red, yellow, green and blue banners | Face north, east, south, west |
| 19 | Chest: the stock | The [storage window](storage.md) |
| 21 | Experience bottle: level and next | The [upgrade window](upgrade.md) |
| 23 | Coal: fuel | The [fuel window](fuel.md) |
| 25 | Hopper: the destination | Cycles where output goes |
| 29 | Anvil: tool durability | A fresh tool, when `tool.durability` is on |
| 30 | Leather chestplate: the skin | The [skin window](skin.md) |
| 31 | Player head | The [trusted window](trusted.md) |
| 32 | Compass: facing | Left: turn clockwise. Right: anticlockwise |
| 33 | Ender pearl: the world | Teleports there, for `teleport.cost` |
| 34 | Gold nugget: what is banked | Collects it, when banking is on |
| 40 | Barrier | The [pickup question](pickup.md) |
| 44 | TNT, with the id | Removes the minion; shown only to holders of `uxmminions.admin` |

Rows 0 to 8 and 36 to 44 are a bar of panes.

The status line is the one that says a minion works only while its owner is here, which turns a surprise into a rule
the player was told.

## Per type overrides

One panel, with only what one type draws differently under `overrides`, keyed by the type's file name:

| Type | Slot 19 shows |
|---|---|
| `seller` | A gold ingot with what it earned, instead of storage |
| `spawner` | A spawner, about the spawner it drives |
| `feeder` | Slot 25 is a bucket about feeding, instead of the destination |

## Actions

`minions:storage`, `minions:upgrade`, `minions:fuel`, `minions:skin`, `minions:trust`, `minions:destination`,
`minions:rotate-left`, `minions:rotate-right`, `minions:face:<north|east|south|west>`, `minions:teleport`,
`minions:pickup`, `minions:admin-remove`, `minions:collect`, `minions:tool`, and `close`, `open:<menu>`,
`command:<line>`, `message:<line>`, `sound:<name> <volume> <pitch>`.

Related: [Placing and using](../features/placing-and-using.md)
