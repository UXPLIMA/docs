---
title: Menu files
order: 608
description: The keys of a window file, overrides, gates, and every source and action the windows know.
icon: layout-template
---

| Key | Meaning |
|---|---|
| `title`, `rows`, `open-actions` | As every UXM window |
| `refresh` | `enabled`, `interval-ticks`: redraw from what was last read |
| `open-requirement` | Conditions to open the window at all |
| `layout`, `layout-char` | Rows of characters, and the character an item fills |
| `slot`, `slots`, `priority`, `type` | Where, which wins, `previous` or `next` |
| `click` | `left`, `right`, `shift-left`, `shift-right`; a click may carry `requirements`, `click` and `deny` |
| `view` | Conditions that decide whether an item is drawn |
| `list` | `source` and `template` |
| `overrides` | Panel only: per type changes, keyed by the type's file name |

## Conditions

| Condition | True when |
|---|---|
| `minions:permission:<node>` | The viewer holds the node |
| `minions:money:<op> <amount>` | The balance compares; `minions:money:coins >= 40` names a pool. Takes nothing |

## Sources

`minions:owned`, `minions:stored`, `minions:types`, `minions:stock`, `minions:requirements`, `minions:fuel-tiers`,
`minions:skins`, `minions:trusted`, `minions:of-player`.

## Actions

| Window | Actions |
|---|---|
| Panel | `minions:storage`, `upgrade`, `fuel`, `skin`, `trust`, `destination`, `rotate-left`, `rotate-right`, `face:<way>`, `teleport`, `pickup`, `admin-remove`, `collect`, `tool` |
| Your minions, admin | `minions:open`, `minions:teleport`, `minions:admin-remove` |
| Pocket | `minions:take-out` |
| Store | `minions:buy`, `minions:buy:<n>` |
| Storage | `minions:take-one`, `take-all`, `drop-all`, `toggle-item`, `storage-row`, `storage-mode`, `sort` |
| Upgrade | `minions:buy-level`, `minions:consume` |
| Fuel, skin, trusted | `minions:refuel`, `minions:wear`, `minions:untrust` |
| Pickup | `minions:pickup-confirm` |
| Any | `minions:panel`, `close`, `open:<menu>`, `command:<line>`, `message:<line>`, `sound:<name> <volume> <pitch>` |

Related: [Menus](../menus/overview.md)
