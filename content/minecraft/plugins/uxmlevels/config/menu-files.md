---
title: Menu files
order: 605
description: Every key a window file takes, the tile line, lists, clicks and actions, and changing a shipped window.
icon: layout-template
---

Every window is a file in `menus/`: `main.conf`, `rewards.conf`, `prestige.conf` and `top.conf`. Move a slot, change a
material, take a fact off a tile, and the plugin draws what you wrote; nothing about a window's layout is decided in
the code. `/uxmlevels reload` reads the files again. A file that is missing or cannot be read falls back to the window
shipped in the jar, and the console says why.

The windows are described on their own pages: [Levels](../menus/main.md), [Rewards](../menus/rewards.md),
[Prestige](../menus/prestige.md), [Top players](../menus/top.md).

## The window

| Key | Meaning |
|---|---|
| `title` | The window title: `@key` for a line of the messages files, or your own words |
| `rows` | Chest height, `1` to `6` |
| `open-actions` | Actions run when the window opens, such as its sound |
| `close-actions` | Actions run when it closes |
| `placeholders { }` | Values a tile reads as `%name%`, written by you |
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
| `name` | The tile's name: `@key`, your own words, or `" "` for a tile whose title is drawn by its lore |
| `lore` | Lines under the name, or one tile line (below) |
| `type` | `previous` or `next` turns the page of the window's list; leave it out for a plain tile |
| `view` | Conditions that hide the tile unless they hold, such as `["levels:nobody"]` or `["has-next"]` |
| `click { }` | What each gesture does |
| `list { }` | Fills the tile's slots from a list the plugin provides |
| `decor { }` | The look of the item: see below |
| `update` | `true` draws the tile again on every refresh |

## The tile line

Every shipped tile is a blank name and one lore line that draws the whole tooltip in the house shape: a title, the
category under it, a description, the facts, and the lines that say what a click does.

```hocon
name = " "
lore = ["tile:%track_position% @menu.track level progress multiplier prestige"]
```

| Part | Meaning |
|---|---|
| `tile:<colour>` | The colour of the title: a gradient of [theme.conf](theme.md), a role such as `good` or `muted`, or a place on the colour wheel as a number |
| `@<key>` | The block of the messages files the words come from: its `title`, `crumb`, `description` and `action` |
| `<fact> ...` | The rows of that block to draw under *Details*, in this order. Take a word off and the tile shows one fact fewer |
| `-action` | Leaves the click lines off |
| `state:<fact>` | Draws a fact as a state, with the theme's `status` glyph |

A `%token%` is filled in first, which is how `tile:%track_position%` gives each track its own colour from the wheel
in the order the tracks are listed. The words stay in the messages files, so moving a tile never
touches a translation.

## Clicks

```hocon
click {
  left  = ["levels:rewards", "sound:block.note_block.pling 0.6 1.5"]
  right = ["levels:top", "sound:block.note_block.pling 0.6 1.5"]
}
```

The gestures are `left`, `right`, `shift_left`, `shift_right`, `middle`, `drop`, `control_drop`, `double_click` and
`any`; a hyphen works in place of the underscore. A gesture with nothing written does nothing. Each is a list of actions, run in order.

| Action | Does |
|---|---|
| `close` | Closes the window |
| `open:<window>` | Opens another window by its file name without `.conf`: `open:top` |
| `command:<line>` | Runs a command as the player, without the slash |
| `message:<line>` | Sends your own words to the player, in MiniMessage |
| `sound:<name> <volume> <pitch>` | Plays a sound, by its key such as `block.note_block.pling` |
| `levels:rewards` | From a track tile: opens the rewards of that track |
| `levels:top` | From a track tile: opens the top players of that track |
| `levels:claim` | From a reward tile: claims that reward |
| `levels:prestige` | From a track tile in `prestige.conf`: prestiges that track |

An action the window does not know is reported in the console when the file is read.

## Lists

```hocon
standings {
  slots = ["10-16", "19-25", "28-34"]
  priority = 5
  list {
    source = "levels:standings"
    template {
      material = "head:%player_id%"
      name = " "
      lore = ["tile:value @menu.top.tile level experience prestige"]
    }
  }
}
```

The plugin fills the slots with one page of entries, row by row; the number of slots is the size of a page. A
`previous` and a `next` tile turn the pages. They are drawn on every page and do nothing where there is no page to go
to; write `view = ["has-next"]` or `view = ["has-previous"]` on one to show it only when it turns a page.

| Source | Window | Entries |
|---|---|---|
| `levels:tracks` | `main.conf`, `prestige.conf` | Every track, in file order |
| `levels:rewards` | `rewards.conf` | The rewards of the track the window was opened for, by level |
| `levels:standings` | `top.conf` | The players ahead on that track, from the snapshot |

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
| `glow` | `true` for the enchantment shine; may be a token such as `%enabled%` |
| `model-data`, `item-model` | A resource pack's model |
| `hide-tooltip` | `true` hides the whole tooltip, used on the filler panes |
| `hide-vanilla-tooltip` | Hides the lines the client writes under the lore; on by default |
| `leather-color` | A dye for leather, as `#rrggbb` |
| `enchantments`, `flags` | Enchantments to show, and item flags |

## Notes

- **A file that does not parse** is reported in the console and the shipped window is drawn until it is fixed.
  `/uxmlevels doctor` counts the windows that were read.
- **A slot past the window's last** is dropped.
- **Keep a filler.** The grey pane under every free slot is what makes a handful of icons read as a menu.

Related: [Menus](../menus/overview.md), [Messages](messages.md), [theme.conf](theme.md)
