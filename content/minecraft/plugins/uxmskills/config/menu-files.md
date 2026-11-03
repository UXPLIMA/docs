---
title: Menu files
order: 507
description: The keys of a window file, sources, conditions and actions.
icon: layout-template
---

| Key | Meaning |
|---|---|
| `title` | `@menu.<window>.title` |
| `rows` | One to six |
| `slot`, `slots` | Where an item sits |
| `source` | Which list fills the slots |
| `view` | Conditions that decide whether an item is drawn |
| `click` | `left`, `right`, `shift-left`, `shift-right` |
| `lore` | One tile: `tile:<colour> @<block> <facts>` |

The plugin's sources, conditions and actions start `skills:` and are listed on each [window page](../menus/overview.md).
The engine adds `close`, `open:<menu>`, `command:<line>`, `message:<line>` and `sound:<name> <volume> <pitch>`.

Related: [Menus](../menus/overview.md)
