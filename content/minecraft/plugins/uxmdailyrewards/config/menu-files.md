---
title: Menu files
order: 503
description: Every key the window file takes, the calendar's own placeholders, list and click actions, and changing the shipped window.
icon: layout-template
---

The window is a file, `menus/calendar.conf`. Move a slot, change a material, change the icon of a state, and the
plugin draws what you wrote. `/uxmdailyrewards reload` reads it again. A file that cannot be read falls back to the
window shipped in the jar, and the console says why; a file you delete is written back at the next start.

## The window

| Key | Shipped | Meaning |
|---|---|---|
| `title` | `"@menu.title"` | The window title: `@key` for a line of the messages files, or your own words |
| `rows` | `6` | Chest height, `1` to `6` |
| `open-actions` | `["sound:ITEM_BOOK_PAGE_TURN 0.7 1.2"]` | Actions run when the window opens |
| `close-actions` | | Actions run when it closes |
| `placeholders { }` | the icons and colours of the four states | Values a tile reads as `%name%` |
| `fill-item`, `click-cooldown`, `refresh { }`, `chest-only` | | As in every UXM window |

## The calendar's placeholders

| Key | Shipped | Meaning |
|---|---|---|
| `icon-claimable` | `"CHEST_MINECART"` | The icon of the ready day |
| `icon-waiting` | `"COMMAND_BLOCK_MINECART"` | The icon of the next day |
| `icon-locked` | `"FURNACE_MINECART"` | The icon of a later day |
| `icon-claimed` | `"MINECART"` | The icon of a taken day |
| `role-claimable` | `"cta"` | The [theme](theme.md) role of the ready day's title |
| `role-waiting` | `"info"` | The next day's |
| `role-locked` | `"muted"` | A later day's |
| `role-claimed` | `"good"` | A taken day's |

An empty `icon-<state>` draws the days of that state with the icon of their day file.

## A tile

Each entry under `items { }` is one tile, keyed by a name you choose.

| Key | Meaning |
|---|---|
| `slot`, `slots` | Where it sits: a number, or a list mixing numbers and ranges such as `"10-16"` |
| `priority` | Which tile wins a slot two of them ask for; the highest is drawn last |
| `material` | The item: a material name, or a `%token%` |
| `name` | The tile's name: `@key`, your own words, or `" "` |
| `lore` | Lines under the name |
| `type` | `previous` or `next` turns the page of the window's list |
| `view` | Conditions that hide the tile unless they hold, such as `["has-next"]` |
| `click { }` | What each gesture does |
| `list { }` | Fills the tile's slots from a list the plugin provides |
| `decor { }` | `amount`, `glow`, `model-data`, `item-model`, `hide-tooltip`, `hide-vanilla-tooltip`, `leather-color`, `enchantments`, `flags` |

## The day list

```hocon
days {
  slots = ["10-16", "19-25", "28-34", "37-43"]
  priority = 5
  list {
    source = "dailyrewards:days"
    template {
      material = "%day_icon%"
      name = "<%day_role%>%day_title%"
      lore = ["%tile_day%"]
      decor {
        glow = "%day_glow%"
        model-data = "%day_model%"
      }
      click { left = ["dailyrewards:claim", "sound:BLOCK_NOTE_BLOCK_PLING 0.6 1.5"] }
    }
  }
}
```

`dailyrewards:days` is every day of the calendar, one per slot, a page at a time; the number of slots is the size of
a page. The tokens a day tile reads:

| Token | Becomes |
|---|---|
| `%day%` | The day's number |
| `%day_title%` | `menu.day.title` for the day |
| `%day_icon%` | The state's icon, or the day file's when the state's is empty |
| `%day_role%` | The state's role |
| `%day_glow%` | `true` for the ready day or a day whose file glows |
| `%day_model%` | The day file's `model-data` |
| `%tile_day%` | The whole tooltip of the day |

`%tile_info%` is the tooltip of the streak tile.

## Clicks

The gestures are `left`, `right`, `shift_left`, `shift_right`, `middle`, `drop`, `control_drop`, `double_click` and
`any`; a hyphen works in place of the underscore. Each is a list of actions, run in order.

| Action | Does |
|---|---|
| `dailyrewards:claim` | Claims the day clicked, when it is the ready day; any other day refuses the click |
| `close` | Closes the window |
| `open:<window>` | Opens another window by its file name |
| `command:<line>` | Runs a command as the player, without the slash |
| `message:<line>` | Sends your own words to the player |
| `sound:<name> <volume> <pitch>` | Plays a sound |

## Notes

- **A slot past the window's last** is dropped.
- **More than 28 days** fill a second page; the arrows in slots 45 and 53 turn it.
- **Keep a filler.** The grey pane under every free slot is what makes the days read as a calendar.

Related: [Calendar window](../menus/calendar.md), [Messages](messages.md), [theme.conf](theme.md)
