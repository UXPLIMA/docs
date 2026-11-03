---
title: Menu files
order: 706
description: Every key a window file takes, the tile line, the lists, clicks and actions.
icon: layout-template
---

Every window is a file in `menus/`: `wardrobe.conf`, `category.conf`, `boxes.conf` and `roulette.conf`. The words are
in the messages files; the layout is here. `/uxmcosmetics reload` reads the files again.

## The window

| Key | Meaning |
|---|---|
| `title` | `@key` for a messages line, or your own words |
| `rows` | `1` to `6` |
| `open-actions` | Actions run when the window opens |
| `fill-item` | One item painted into every slot nothing else drew |

## A tile

| Key | Meaning |
|---|---|
| `slot`, `slots` | A number, or a list mixing numbers and ranges such as `"10-16"` |
| `priority` | Which tile wins a slot two of them ask for |
| `material` | A material, or `cosmetic` / `box` in a list template for the entry's own icon |
| `name` | `@key`, your own words, or `" "` for a tile whose title is drawn by its lore |
| `lore` | Lines, or one tile line |
| `type` | `previous` or `next` |
| `view` | Conditions that hide the tile, such as `["cosmetics:empty"]` |
| `click { }` | `left`, `right`, `shift_left` and more, each a list of actions |
| `list { source, template }` | Fills the slots from a list |
| `decor { }` | `amount`, `glow` (may be `%worn%`), `model-data`, `item-model`, `hide-tooltip` |

## The tile line

```hocon
lore = ["tile:0 @menu.wardrobe.trail owned"]
```

| Part | Meaning |
|---|---|
| `tile:<colour>` | The title's colour: a wheel place, a gradient or a role; `%colour%` is the family's or tier's |
| `@<key>` | The block of the messages files the words come from: `title`, `crumb`, `description`, `action` |
| `<fact> ...` | The rows of that block to draw under *Details*, in order |

`%cosmetic_state%` in a key picks the block of the piece's state: `worn`, `owned`, `played`, `for-sale` or
`not-for-sale`.

## Tokens

`%owned_<family>%`, `%total_<family>%`, `%name%`, `%description%`, `%rarity%`, `%price%`, `%state%`, `%action%`,
`%category%`, `%colour%`, `%worn%`, `%cosmetic_state%`.

## Lists

| Source | Window | Entries |
|---|---|---|
| `cosmetics:of-category` | `category.conf` | The pieces of the family the window was opened for |
| `cosmetics:all-boxes` | `boxes.conf` | Every box |

## Actions

Besides `close`, `open:<window>`, `command:<line>`, `message:<line>` and `sound:<name> <volume> <pitch>`:

| Action | Does |
|---|---|
| `cosmetics:open:<family>` | Opens a family window |
| `cosmetics:wardrobe` | Opens the wardrobe |
| `cosmetics:boxes` | Opens the boxes window |
| `cosmetics:unequip-all` | Takes everything off |
| `cosmetics:use` | From a piece: wears, takes off, plays or buys it |
| `cosmetics:open-box` | From a box: opens it |

## roulette.conf

Not drawn by the menu engine: the plugin fills it. The file still gives the `title`, the `rows`, the `strip` tile's
slots and the filler.

Related: [Menus](../menus/overview.md), [Messages](messages.md)
