---
title: Menu files
order: 505
description: The keys of a window file, tiles, sources, conditions and actions.
icon: layout-template
---

| Key | Meaning |
|---|---|
| `title` | `@menu.<window>.title`, or your own words |
| `rows` | One to six |
| `slot`, `slots` | Where an item sits; a range is `"0-8"`, and a list mixes both |
| `priority` | Which item wins a slot two ask for; the highest is drawn last |
| `type` | `previous` or `next` turns the page |
| `click` | `left`, `right`, `shift_left`, `shift_right` |
| `source` | Which list fills the slots |
| `view` | Conditions that decide whether an item is drawn |

A tile is a tooltip in six blocks: a title, the category under it, a description, the facts, and the click line,
written as a blank name and one lore line:

```hocon
name = " "
lore = ["tile:5 @menu.quests.group done"]
```

The word after `tile:` is the title's colour: a gradient from theme.conf, a role, or a place on the colour wheel as a
number. The key after it names a block of the messages, and every word after names a fact, in the order drawn.

| Name | Kind | Window |
|---|---|---|
| `quests:groups`, `quests:board` | Source | Quests, board |
| `quests:passes`, `quests:tiers` | Source | Passes, pass |
| `quests:phase`, `quests:tier-state` | Condition | Board, pass |
| `quests:open`, `quests:claim`, `quests:start`, `quests:reroll`, `quests:back` | Action | Quests, board |
| `quests:pass-open`, `quests:pass-claim`, `quests:pass-back` | Action | Passes, pass |

Every window also knows `close`, `open:<menu>`, `command:<line>`, `message:<line>` and
`sound:<name> <volume> <pitch>`.

Related: [Menus](../menus/overview.md), [Messages](messages.md)
