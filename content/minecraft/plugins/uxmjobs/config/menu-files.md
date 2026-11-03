---
title: Menu files
order: 512
description: The keys of a window file, view lists, and every source, condition and action the windows know.
icon: layout-template
---

Every window in `menus/` is a file: move a slot, change a material, take a button out, and the plugin draws what you
wrote.

| Key | Meaning |
|---|---|
| `title` | `@menu.title.<window>`, or your own words |
| `rows` | One to six |
| `open-actions` | What runs when it opens |
| `layout`, `layout-char` | Rows of characters, and the character an item fills |
| `slot`, `slots` | Where an item sits |
| `priority` | Which item wins a shared slot |
| `type` | `previous` or `next` |
| `click` | `left`, `right`, `shift_left`, `shift_right`; a click may carry `requirements`, `click` and `deny` |
| `view` | Conditions that decide whether an item is drawn; `!` reverses one |
| `list` | `source` and `template` |

`lore = ["tile:<colour> @<block> <facts>"]` draws a tile; a fact named `-action` drops the click line.

## Sources, conditions and actions

| Name | Kind | Window |
|---|---|---|
| `jobs:all`, `jobs:actions`, `jobs:rewards`, `jobs:top`, `jobs:boosters` | Source | Board, job, levels, leaderboard, boosters |
| `jobs:contracts`, `jobs:signed`, `jobs:shop`, `jobs:report`, `jobs:editor-list` | Source | Contracts, shop, report, editor |
| `jobs:held`, `jobs:can-join`, `jobs:contract-signed` | Condition | Board, contract |
| `jobs:join`, `jobs:leave` | Action | Board, leave |
| `jobs:open-job`, `jobs:open-quit`, `jobs:open-levels`, `jobs:open-contract` | Action | Opens that window |
| `jobs:sign`, `jobs:cancel` | Action | Contract |
| `jobs:buy` | Action | Shop |
| `jobs:editor-properties`, `jobs:editor-new` | Action | Editor |

Every window also knows `close`, `open:<menu>`, `command:<line>`, `message:<line>` and
`sound:<name> <volume> <pitch>`.

Related: [Menus](../menus/overview.md), [Messages](messages.md)
