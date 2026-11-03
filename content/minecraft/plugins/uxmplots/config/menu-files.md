---
title: Menu files
order: 515
description: The keys of a window file, conditions and actions, and the two editor layouts.
icon: layout-template
---

| Key | Meaning |
|---|---|
| `title` | `@menu.<window>.title`, or your own words |
| `rows` | One to six |
| `slot`, `slots` | Where an item sits; `slots` takes ranges such as `"0-44"` |
| `priority` | Which item wins a shared slot |
| `material` | A material, or `skull:<name>` |
| `name`, `lore` | `lore = ["tile:<colour> @<block> <facts>"]` draws a tile |
| `view` | Conditions that decide whether an item is drawn; `!` reverses one |
| `click` | `left`, `right`, `shift_left`, `shift_right`; a click may carry `requirements` and `actions` |
| `list` | `template` for the rows of a list |
| `decor` | `glow`, `hide-tooltip` |

Every window knows `close`, `open:<menu>`, `command:<line>`, `message:<line>`, `sound:<name> <volume> <pitch>`, and
`has-previous`, `has-next`. List windows know `list-sort:plots:<window>:next|previous`,
`list-filter:plots:<window>:<key>=<value>` and `list-search:plots:<window>:search:@<prompt>`. The plugin's own
conditions and actions start `plots:` and are listed on each [window page](../menus/overview.md).

## Editor layouts

`editor/world.conf` and `editor/contest.conf` lay out the in-game editors' buttons. They hold no words: every label is a
message key. A number button takes `step`, `shift`, `min` and `max`. Delete one and the editor returns to the shipped
shape.

Related: [Menus](../menus/overview.md), [Messages](messages.md)
