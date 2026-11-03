---
title: Menu files
order: 614
description: The keys of a window file, the tokens and actions each window knows, and the two editor layouts.
icon: layout-template
---

Every window in `menus/` is a file. Move a slot, change a material, take a button out, and the plugin draws what you
wrote. Nothing about a window is decided in the code.

## The keys

| Key | Meaning |
|---|---|
| `title` | A key of the messages files written with a leading `@`, or your own words |
| `rows` | One to six |
| `slot`, `slots` | Where an item sits; a range is `"10-16"`, and a list mixes both |
| `priority` | Which item wins a slot two of them ask for; the highest is drawn last |
| `type` | `previous` or `next` turns the page of the list |
| `click` | What a `left`, `right`, `shift-left` or `shift-right` click does |
| `list` | The squares a list sits in, its `source` and the `template` of each square |

```hocon
settings {
  slots = ["10-16", "19-25"]
  priority = 5
  list {
    source = "chat:settings"
    template {
      material = "%setting_icon%"
      name = " "
      lore = ["%tile_setting%"]
      decor { glow = "%setting_on%" }
      click { left = ["chat:setting-flip"] }
    }
  }
}
```

Every window also knows `close`, `open:<menu>`, `command:<line>`, `message:<line>` and
`sound:<name> <volume> <pitch>`.

## Tokens and actions

| Window | Tokens | Actions |
|---|---|---|
| `settings.conf` | `%setting_icon%`, `%setting_on%`, `%tile_setting%` | `chat:setting-flip`: turns the setting on or off, or picks the next mention style |
| `recent.conf` | `%tile_recent%` | `chat:recent-answer`, `chat:recent-ignore` |
| `friends.conf` | `%tile_friend%` | `chat:friend-message`, `chat:friend-pin` |
| `emojis.conf` | `%tile_emoji%` | `chat:emoji-copy` |
| `colours.conf` | `%colour_icon%`, `%colour_worn%`, `%tile_colour%`, `%style_on%`, `%tile_style%`, `%tile_colour_clear%` | `chat:colour-pick`, `chat:colour-style`, `chat:colour-clear` |
| `tag-categories.conf` | `%category_icon%`, `%tile_category%`, `%tile_tag_clear%` | `chat:tag-category`, `chat:tag-clear` |
| `tags.conf` | `%tag_icon%`, `%tag_worn%`, `%tile_tag%` | `chat:tag-click`, `chat:tag-pin` |
| `editor.conf` | `%editor_icon%`, `%tile_editor%` | `chat:editor-open`, `chat:editor-new` |

The list sources are `chat:settings`, `chat:recent`, `chat:friends`, `chat:emojis`, `chat:colour-presets`,
`chat:colour-styles`, `chat:tag-categories`, `chat:tags` and `chat:editor-list`.

## The editor layouts

`editor-list.conf` and `editor-properties.conf` take keys rather than items:

| File | Keys |
|---|---|
| `editor-list.conf` | `rows`, `entry-slots`, `add-slot`, `back-slot`, `entry-icon`, `add-icon`, `back-icon`, `filler` |
| `editor-properties.conf` | `rows`, `property-slots`, `back-slot`, `delete-slot`, `back-icon`, `delete-icon`, `filler` |

Related: [Menus](../menus/overview.md), [Messages](messages.md)
