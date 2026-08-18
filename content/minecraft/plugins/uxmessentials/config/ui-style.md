---
title: UI Style
order: 1190
description: The palette, glyphs and text formats the built-in messages and menus follow.
icon: palette
---

uxmEssentials ships with one consistent look across chat, command feedback, GUIs and the
HUD. When you edit [messages](messages.md) or build [custom menus](../menus/engine.md),
following the same conventions keeps your server looking like one plugin instead of a
patchwork. This page is the practical summary for operators.

**HOCON, not YAML.** Message catalogs and menu files are HOCON: quoted `"key" = "value"` lines and
`{ … }` blocks. The styling below is written in MiniMessage tags inside those values.

## Style tokens, not raw colours

The plugin defines a fixed palette once, in code, and exposes it as **semantic tags**.
Instead of writing a hex code, you name the *role* the text plays. This is why editing one
message never clashes with the rest of the UI: every line pulls from the same palette.

| Token | Use it for | Colour |
|---|---|---|
| `<accent>` | Brand accent, section labels, navigation | sky `#38B6FF` |
| `<value>` | A name, an amount, a number, a duration | ice `#8FD9FF` |
| `<body>` | The sentence itself and field labels | white |
| `<subtext>` | Lore description lines | `#DDE8F0` |
| `<muted>` | Dates, counters, parentheticals | `#93A4B3` |
| `<dim>` | Separators and disabled state | `#6B7886` |
| `<icon>` | The glyph at the head of a lore line | `#8A93A1` |
| `<crumb>` | The breadcrumb under a lore title | `#565F6B` |
| `<good>` | Success, enabled, incoming money | green `#5BE38C` |
| `<bad>` | Error, denial, disabled | red `#FF6B6B` |
| `<money>` `<level>` `<cta>` | An amount, a highlighted number, the click word | gold `#FFC93C` |
| `<info>` | A section header inside lore | cyan `#4FD6E8` |
| `<rank>` `<event>` | Rank tiers, events and bonuses | purple, pink |

The solid-colour tokens wrap their content (`<value>Steve</value>`). The composite tokens
take one quoted argument and are self-closing:

| Token | Renders |
|---|---|
| `<tag:'HOME'>` | The category prefix: the label bold in small capitals, then a dim `▶` |
| `<etag:'…'>` | The red `ᴇʀʀᴏʀ` word and the dim `▶` |
| `<h:'Home Panel'>` | A bold sky header, written in small capitals |

A category prefix is sky unless the category is about money (`ECONOMY`, `BANK`, `LOAN`, `TRADE`,
`VAULT`), where it turns green so a balance line is recognisable before it is read.

## Small capitals

Fixed text is written in small capitals: `ᴀʙᴄᴅᴇꜰɢʜɪᴊᴋʟᴍɴᴏᴘǫʀꜱᴛᴜᴠᴡxʏᴢ`. Note that `x` has no
small-capital form and stays as it is, and `q` becomes `ǫ`.

Digits, symbols, `{placeholders}`, `%papi%` tokens, tag names and `/command` literals stay in
ordinary characters, so a player can read a number and type a command. A player's own name arrives
as they spell it.

The argument of `<tag:>`, `<etag:>` and `<h:>` is written in plain ASCII: the tag converts it when it
renders, so you never type the glyphs by hand.

<Callout type="danger" title="Never inline hex or legacy codes">

Don't write `<#8fd9ff>`, `<gradient:#…>`, `&a`, `§a` or a named colour like `<red>` in a
restyled line. Reference a token instead. A raw hex in a catalog is treated as a regression
and flagged by the build's drift checks.

Also never put a `{placeholder}` **inside** a tag argument (e.g. `<h:'Home {home}'>`):
a value containing a quote or angle bracket would break the tag. Put dynamic values
outside the argument: `<h:'Home'> <value>{home}</value>`.

</Callout>

## Chat lines

Lead with a prefix, colour the parts by role, and stop there. A chat line carries **no** trailing
tick, cross or emoji: a red `ᴇʀʀᴏʀ ▶` already says the action failed.

```hocon
"pay.sent" = "<tag:'ECONOMY'> <body>ʏᴏᴜ ᴘᴀɪᴅ</body> <money>{amount}</money> <body>ᴛᴏ</body> <value>{player}</value>."
"command.no-permission" = "<etag:'ERROR'> <bad>ʏᴏᴜ ᴅᴏɴ'ᴛ ʜᴀᴠᴇ ᴘᴇʀᴍɪꜱꜱɪᴏɴ.</bad>"
```

The category word does not repeat in the body, and at most two accent colours appear in one line.
A key that is embedded inside another message (a currency format, a list row, a help entry) carries
no prefix at all.

## GUI item lore

Every item carries one block, and the block opens with its title. The plugin writes that title line
itself, from the item's own name, and leaves the display name blank: that hides the material name and
puts a line of space above the title. It closes the block with a blank line too, so the last line has
the same air under it. A lore block you write therefore starts at the breadcrumb, one line below, and
stops at its last written line. The order of the lines is fixed:

```text
 ◆ ʜᴏᴍᴇ                          (written for you, from the item's name)
    ᴏɴᴇ ᴏꜰ ʏᴏᴜʀ ꜱᴀᴠᴇᴅ ʜᴏᴍᴇꜱ

 ✎ ᴅᴇꜱᴄʀɪᴘᴛɪᴏɴ
    ᴀ ᴘʟᴀᴄᴇ ʏᴏᴜ ꜱᴀᴠᴇᴅ, ᴏɴᴇ ᴄʟɪᴄᴋ ᴀᴡᴀʏ.

 ≡ ɪɴꜰᴏʀᴍᴀᴛɪᴏɴ
    • ᴡᴏʀʟᴅ world
    • ᴀᴛ 120, 64, -32

 → ᴄʟɪᴄᴋ ᴛᴏ ᴍᴀɴᴀɢᴇ ᴛʜɪꜱ ʜᴏᴍᴇ
                                 (written for you: the closing blank line)
```

Written out, that is one `<newline>`-joined string:

```hocon
"    <crumb>ᴏɴᴇ ᴏꜰ ʏᴏᴜʀ ꜱᴀᴠᴇᴅ ʜᴏᴍᴇꜱ</crumb><newline><newline> <icon>✎</icon> <info>ᴅᴇꜱᴄʀɪᴘᴛɪᴏɴ</info><newline>    <subtext>ᴀ ᴘʟᴀᴄᴇ ʏᴏᴜ ꜱᴀᴠᴇᴅ, ᴏɴᴇ ᴄʟɪᴄᴋ ᴀᴡᴀʏ.</subtext><newline><newline> <icon>≡</icon> <info>ɪɴꜰᴏʀᴍᴀᴛɪᴏɴ</info><newline>    <icon>•</icon> <body>ᴡᴏʀʟᴅ</body> <value>{world}</value><newline><newline> <icon>→</icon> <cta>ᴄʟɪᴄᴋ</cta> <subtext>ᴛᴏ ᴍᴀɴᴀɢᴇ ᴛʜɪꜱ ʜᴏᴍᴇ</subtext>"
```

Rules that are easy to get wrong:

- The title is drawn bold, in a gradient. Its family follows the colour you gave the item's name: a
  delete stays red, money stays green, a call to act stays gold, and anything else reads in the
  brand blue. You never need to colour a title yourself.
- `✎` is the description header and `≡` is the information header. Don't swap them.
- The description header is mandatory: body lines never hang under the breadcrumb.
- Every line opens and closes with one space.
- A state is a coloured **word** (`ᴇɴᴀʙʟᴇᴅ` green, `ᴅɪꜱᴀʙʟᴇᴅ` red), never a tick or cross.
- An item that carries information carries at least six filled lines.

## Titles, navigation and sound

**Titles** are centred and bare: no colour, no bold, no dashes around them. The plugin centres each
one and strips whatever styling it finds, so a title key is written as plain text with no tag at all.

**Navigation buttons** are an `ARROW` with a single-line name and no lore:

```hocon
"home.menu.prev"      = "<accent>← ᴘʀᴇᴠɪᴏᴜꜱ ᴘᴀɢᴇ</accent>"
"home.menu.next"      = "<accent>→ ɴᴇxᴛ ᴘᴀɢᴇ</accent>"
"home.menu.page-info" = "<muted>ᴘᴀɢᴇ</muted> <value>{page}</value><dim>/</dim><value>{max_page}</value>"
```

**Sound** is part of the look. Every menu declares an open sound through the engine's
`sound:<key> <volume> <pitch>` action on `open-actions`. Click feedback is automatic: the engine
plays it for every gesture it accepts, so your own menus get it without writing anything. A slot
with nothing bound to it stays silent, and a click that declares its own `sound:` effect replaces
the default rather than stacking on top of it.

| Event | Sound | volume / pitch |
|---|---|---|
| Menu opens | `ITEM_BOOK_PAGE_TURN` | 0.7 / 1.2 |
| Confirm, positive | `BLOCK_NOTE_BLOCK_PLING` | 0.6 / 1.5 |
| Cancel, danger | `BLOCK_NOTE_BLOCK_BASS` | 0.6 / 0.9 |
| Pagination | `ITEM_BOOK_PAGE_TURN` | 0.7 / 1.0 |
| A requirement refused the click | `BLOCK_NOTE_BLOCK_BASS` | 0.6 / 0.9 |
| Money | `BLOCK_NOTE_BLOCK_BELL` | 0.5 / 1.5 |
| Anything else | `UI_BUTTON_CLICK` | 0.5 / 1.6 |

Filler panes stay silent. Keep volumes low: a menu is clicked far more often than anything else in
the game.

## Glyph legend

These are plain Unicode characters (no resource pack). Each one has one meaning:

| Glyph | Meaning |
|---|---|
| `▶` | The separator after a chat prefix, and nothing else |
| `◆` | The title line of a lore block (written for you) |
| `✎` | The description header |
| `≡` | The information header |
| `•` | A bullet inside an information row |
| `→` | An action line, and the next-page button |
| `←` | The previous-page and back buttons |

Emoji are not used anywhere. If a fact needs marking, it is marked by colour and by a word.

## Styling menus

Custom menus in `plugins/uxmEssentials/menus/*.conf` are styled with the **same
MiniMessage tags**. Titles, item names, and lore in a menu file are written **verbatim**:
they are not routed through the message catalogs, so you can style them however you like,
though matching the conventions above keeps them consistent with the built-in GUIs.

```hocon
# menus/example.conf
title = "Example Menu"
rows = 3
open-actions = ["sound:ITEM_BOOK_PAGE_TURN 0.7 1.2"]
items {
  spawn {
    slot = 11
    material = COMPASS
    name = ""
    lore = ["    <crumb>ꜱᴇʀᴠᴇʀ ʟᴏᴄᴀᴛɪᴏɴ</crumb><newline><newline> <icon>→</icon> <cta>ᴄʟɪᴄᴋ</cta> <subtext>ᴛᴏ ᴛᴇʟᴇᴘᴏʀᴛ</subtext>"]
    click { left = ["command:spawn", "close"] }
  }
}
```

The built-in module GUIs (the `modules/<m>/gui/*.conf` panels) already follow these
conventions: treat them as worked examples if you are building your own.

## A few rules of thumb

- **Do** reference colours by token and let `<tag:>`, `<etag:>` and `<h:>` do the prefixes and headers.
- **Do** give every menu a deliberate layout: a border, grouped rows, centred content. A default grid
  with new colours still reads as a default grid.
- **Do** pick a vanilla material that says what the item is. Use a head only when the head *is* the
  information (a player entry, an owner).
- **Don't** inline hex or legacy `&`/`§` codes, invent new tokens, or rename `{placeholders}`.
- **Don't** end a chat line with a tick, a cross or an emoji.
- Styling is language-independent: when you restyle a key in one language, apply the same
  token edit to the matching key in every other language file.

Related: [Messages & Languages](messages.md), [Custom Menu Engine](../menus/engine.md), [Built-in Menu Guide](../menus/built-in.md)
