---
title: Overview
order: 401
description: The six window files, who opens them, tiles and the tokens every window knows.
icon: layout-template
---

| Window | File | Opened by | Rows |
|---|---|---|---|
| [Wiki](wiki.md) | `menus/wiki.conf` | `/enchants` | 6 |
| [Grouped wiki](grouped.md) | `menus/grouped.conf` | `/enchants groups`, the bookshelf | 6 |
| [Enchantment page](info.md) | `menus/info.conf` | `/enchants info <enchantment>`, a click on a tile | 3 |
| [Editor](editor.md) | `menus/editor.conf` | `/uxmenchants editor` | 6 |
| [Editor lists](editor.md#lists) | `menus/editor-list.conf` | A list button of the editor | 6 |
| [Property editor](editor.md#properties) | `menus/editor-enchant.conf` | A click on an enchantment in the editor | 6 |

The wiki and the grouped wiki share one shape: panes around the list, the arrows at 48 and 50, a button at 49 and
close at 53.
A Bedrock player through Floodgate gets a native form drawn from the same file.

## Tiles

A tooltip is drawn in six blocks: the title, the category under it, the description, the facts, and the line that says
what a click does. It is written as a blank name and one lore line:

```hocon
name = " "
lore = ["tile:5 @menu.wiki.entry type maximum targets conflicts"]
```

The word after `tile:` is the colour of the title: a gradient of `theme.conf`, a role, or a place on the colour wheel as
a number. The key names a block of the messages file, and every word after it names one fact of that block, in the
order drawn. Take a word off the line and the tooltip draws one fact fewer.

## Tokens

| Token | Value |
|---|---|
| `%enchant_id%` | The id |
| `%enchant_name%`, `%enchant_description%` | In the reader's language |
| `%enchant_rarity%`, `%enchant_type%` | In the reader's language |
| `%enchant_maximum%` | The highest level |
| `%enchant_targets%` | What it goes on |
| `%enchant_conflicts%`, `%enchant_requires%`, `%enchant_removes%` | The three combine lists |
| `%enchant_table%`, `%enchant_villager%`, `%enchant_loot%`, `%enchant_anvil%` | `yes` or `no` for each route |

Every window is a file; see [Menu files](../config/menu-files.md). The sounds of every window are `menu.sounds` in
`config.conf`. A window whose file is deleted is forgotten, and the command that opens it says `menu.missing`; the next
start writes the shipped file back.

Related: [Menu files](../config/menu-files.md)
