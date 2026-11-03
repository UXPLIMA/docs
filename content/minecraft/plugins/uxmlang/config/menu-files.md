---
title: Menu file
order: 504
description: The keys of menus/language.conf, the tile line, the tokens and the actions.
icon: layout-template
---

`menus/language.conf` is the window. Nothing about it is decided in the code.

| Key | Meaning |
|---|---|
| `title` | `@menu.language.title`, or your own words |
| `rows` | `3` |
| `open-actions` | A page-turn sound |
| `items` | `languages` (the list, slots 10 to 16), `bar` (the pane along the bottom), `previous` (18), `follow` (22), `next` (26) |

```hocon
languages {
  slots = ["10-16"]
  priority = 5
  list {
    source = "lang:languages"
    template {
      material = "%language_icon%"
      name = " "
      lore = ["tile:5 @menu.language.tile state"]
      click { left = ["lang:choose", "sound:BLOCK_NOTE_BLOCK_PLING 0.6 1.5"] }
    }
  }
}
```

The number of slots in `languages` is the number of languages one page holds.

## The tile line

`tile:<colour> @<block> <facts>` draws the house tooltip from a block of the messages files: a title, the category under
it, a description, the facts, and the line that says what a click does. The colour is a gradient named in
`theme.conf`, a role, or a place on the colour wheel written as a number.

## Tokens

| Token | Value |
|---|---|
| `%language_icon%` | The icon from `languages.conf` |
| `%language%` | The language's name |
| `%tag%` | Its tag, such as `pt-BR` |
| `%state%` | `Reading now` or `Available`, in the viewer's language |

## Actions

| Name | Does |
|---|---|
| `lang:languages` | The list source: every language the server has |
| `lang:choose` | Reads the server in the language of the clicked row; on a button outside the list it does nothing |
| `lang:reset` | Follows the client again |

Every window also knows `close`, `open:<menu>`, `command:<line>`, `message:<line>` and
`sound:<name> <volume> <pitch>`.

Related: [Language window](../menus/language.md), [Messages](messages.md)
