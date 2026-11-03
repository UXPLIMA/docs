---
title: Menu files
order: 604
description: The keys of a window file, tiles, tokens and actions.
icon: layout-template
---

| Key | Meaning |
|---|---|
| `title` | `@menu.<window>.title` |
| `rows` | One to six |
| `open-actions` | What runs when it opens |
| `slot`, `slots` | Where an item sits |
| `priority` | Which item wins a shared slot |
| `material` | A material, or a token such as `%claim_icon%`, `%member_head%` |
| `lore` | One tile: `tile:<colour> @menu.<block> <facts>` |
| `click` | `left`, `right`, `shift-left`, `shift-right` |
| `source` | Which list fills the slots: `claims:claims`, `claims:members`, `claims:bans`, `claims:roles`, `claims:permissions`, `claims:flags`, `claims:rules`, `claims:areas`, `claims:map`, `claims:market`, `claims:inbox`, `claims:audit` |
| `fill-item` | What fills empty slots |

A row's tile colour may be `%<row>_position%`, so rows shade along the list.

The actions start `claims:` and are listed on each [window page](../menus/overview.md). The engine adds `close`,
`open:<menu>`, `command:<line>`, `message:<line>` and `sound:<name> <volume> <pitch>`.

Related: [Menus](../menus/overview.md)
