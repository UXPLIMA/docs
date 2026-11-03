---
title: Menu files
order: 504
description: Every key a window file takes, the tiles, lists, clicks and actions, and changing a shipped window.
icon: layout-template
---

Every window is a file in `menus/`: `themes.conf`, `arenas.conf` and `arena-editor.conf`. Move a slot, change a
material, take a button out, and the plugin draws what you wrote; nothing about a window's layout is decided in the
code. `/uxmparkour reload` reads the files again. A file you delete is written back from the jar at the next start,
and until then that window does not open.

The three windows are described on their own pages: [Theme picker](../menus/themes.md),
[Arena list](../menus/arenas.md), [Arena editor](../menus/editor.md).

## The window

| Key | Meaning |
|---|---|
| `title` | The window title: `@key` for a line of the messages files, or your own words. Keep a title plain text |
| `rows` | Chest height, `1` to `6` |
| `open-actions` | Actions run when the window opens, such as its sound |
| `close-actions` | Actions run when it closes |
| `placeholders { }` | Values a tile reads as `%name%`. The shipped files use it for the icons and colours of each state |
| `fill-item` | One item painted into every slot nothing else drew |
| `click-cooldown` | Milliseconds two clicks must be apart to count as two |
| `refresh { enabled, interval-ticks }` | Draws the window again on a timer |
| `chest-only` | `true` keeps Bedrock players on the chest instead of a form |

## A tile

Each entry under `items { }` is one tile, keyed by a name you choose.

| Key | Meaning |
|---|---|
| `slot`, `slots` | Where it sits: a number, or a list mixing numbers and ranges such as `"10-16"` |
| `priority` | Which tile wins a slot two of them ask for; the highest is drawn last |
| `material` | The item: a material name, or a `%token%` the window fills in |
| `name` | The tile's name: `@key`, your own words, or a token |
| `lore` | Lines under the name; a `%tile_...%` token is a whole tooltip in the house shape |
| `type` | `previous` or `next` turns the page of the window's list; leave it out for a plain tile |
| `view` | Conditions that hide the tile unless they hold, such as `["has-next"]` on a page arrow |
| `click { }` | What each gesture does |
| `list { }` | Fills the tile's slots from a list the plugin provides |
| `decor { }` | The look of the item: see below |
| `update` | `true` draws the tile again on every refresh |

## Clicks

```hocon
click {
  left          = ["parkour:edit-arena", "sound:UI_BUTTON_CLICK 0.5 1.6"]
  "shift-right" = ["parkour:delete-arena"]
}
```

The gestures are `left`, `right`, `shift-left`, `shift-right`, `middle`, `drop`, `control-drop` and `double-click`. A
gesture with nothing written does nothing. Each is a list of actions, run in order.

| Action | Does |
|---|---|
| `close` | Closes the window |
| `open:<window>` | Opens another window by its file name without `.conf`: `open:arenas` |
| `command:<line>` | Runs a command as the player, without the slash |
| `message:<line>` | Sends your own words to the player, in MiniMessage |
| `sound:<name> <volume> <pitch>` | Plays a sound, by its constant (`UI_BUTTON_CLICK`) or its key |
| `parkour:...` | The window's own actions, listed on its page |

An action the window does not know is reported in the console when the file is read, not on the click that would
have reached it.

## Lists

```hocon
arenas {
  slots = ["10-16", "19-25", "28-34"]
  priority = 5
  list {
    source = "parkour:arenas"
    template {
      material = "%arena_icon%"
      name = "<%arena_role%>%arena%"
      lore = ["%tile_arena%"]
      click { left = ["parkour:edit-arena"] }
    }
  }
}
```

The plugin fills the slots with one page of entries, row by row; the number of slots is the size of a page. A
`previous` and a `next` tile turn the pages. They are drawn on every page and do nothing where there is no page to
go to; write `view = ["has-next"]` or `view = ["has-previous"]` on one to show it only when it turns a page. The sources are `parkour:themes`, `parkour:arenas` and
`parkour:pieces`, each with its own tokens, listed on the window's page.

| Key | Meaning |
|---|---|
| `source` | The list to draw |
| `template` | The tile drawn once per entry, with the entry's tokens |
| `empty { }` | A tile drawn in each slot a page leaves without an entry |
| `align = center` | Centres the short last row of a page |

## decor

| Key | Meaning |
|---|---|
| `amount` | The stack size shown |
| `glow` | `true` for the enchantment shine; may be a token such as `%theme_worn%` |
| `model-data`, `item-model` | A resource pack's model |
| `hide-tooltip` | `true` hides the whole tooltip, used on the filler panes |
| `hide-vanilla-tooltip` | Hides the lines the client writes under the lore, such as armour stats; on by default |
| `leather-color` | A dye for leather, as `#rrggbb` |
| `enchantments`, `flags` | Enchantments to show, and item flags |

## Text in a window

A name, lore line or title that starts with `@` is a line of the messages files, read in the viewer's language. Any
other text is MiniMessage, with the [theme](theme.md) roles such as `<accent>` and `<good>`. A `%token%` is filled
in before the line is read, which is how `<%arena_role%>%arena%` paints an arena's name in the colour of its state.
`{math: ...}` evaluates arithmetic in a line: `{math: %count% * 2}`.

## Notes

- **A file that does not parse** is reported in the console, and that window does not open until it is fixed.
  `/uxmparkour doctor` counts the windows that were read.
- **A slot past the window's last** is dropped.
- **Keep a filler.** The grey pane under every free slot is what makes a handful of icons read as a menu.

Related: [Menus](../menus/overview.md), [Messages](messages.md), [theme.conf](theme.md)
