---
title: Messages
order: 404
description: How a line is written, the roles and prefixes it uses, and every block of keys with the values each fills in.
icon: message-square
---

Every line the plugin writes comes from `messages/messages_<language>.conf`, one file per language. No text is written
in the code, so any line can be rewritten and the plugin translated whole. Run `/uxmjumppads reload` after an edit.

## How a line is written

```hocon
pad {
  removed = "<tag:'JUMP PADS'> <body>The pad under you is gone."
}
```

- The file is HOCON blocks: `pad.removed` is the key `removed` inside the block `pad`.
- A line is [MiniMessage](https://docs.advntr.dev/minimessage/format.html). Legacy `&a` codes are shown as written.
- A line names a **role**, not a colour: `<body>` is the text, `<value>` a name or a number, `<good>`, `<bad>` and
  `<warn>` a result. The colours behind the roles are in [theme.conf](theme.md), shared by every UXM plugin.
- A tag that is not a role is a **value** the plugin fills in, such as `<type>`. The values of each line are fixed;
  they are listed below. A value is inserted as text, never as markup.
- Write ordinary letters. Small capitals are applied when the line is drawn, for the languages
  [theme.conf](theme.md#small-capitals) turns them on for, so a translator reads plain text.
- A line you delete falls back to the default built into the plugin, so a missing key is never blank.

## The prefixes

| Tag | Draws |
|---|---|
| `<tag:'JUMP PADS'>` | The category word in small capitals and the separator glyph, in the category's colour |
| `<etag:'ERROR'>` | The same for an error, in the `bad` colour |
| `<h:'How this plugin is'>` | A header, in the `header` gradient |

A category's colour is set under `prefix.categories` in [theme.conf](theme.md#prefix); `JUMP PADS` is not named there, so
it reads in the accent colour. The argument in quotes is plain text: never put a value inside it.

## The keys

| Key | Values | Said when |
|---|---|---|
| `pad.set` | `<spot>`, `<type>`, `<facing>` | `set` |
| `pad.unknown-type` | `<type>`, `<types>` | `set` with a type `config.conf` does not have |
| `pad.removed` | | `remove` |
| `pad.none-here` | | `remove` where no pad is set |
| `pad.list-header` | `<count>` | `list` |
| `pad.list-entry` | `<type>`, `<spot>`, `<facing>` | One line of `list` |
| `pad.list-empty` | | `list` with no set pad |
| `admin.usage` | | The bare `/uxmjumppads` |
| `admin.version`, `admin.reloaded`, `admin.reload-failed` | `<version>`, `<ms>` | `version` and `reload` |
| `admin.doctor.*` | `<check>`, `<said>` | `doctor` |
| `language.*` | | The replies of `/jumppad lang` and its subcommands |
| `command.*` | | The replies every command shares: no permission, player only, a value that is not valid, the help pages |

A pad's own effects can name any key with `@`, including one you add, so a pad can say something in every language:
add `pad.whee = "<accent>Whee!"` to every file and write `[actionbar] @pad.whee` in the type's effects.

## Notes

- **Edit every language.** A line rewritten in English stays as it was in the other files; change each one you use.

Related: [Languages](languages.md), [theme.conf](theme.md), [Effects](../pads/effects.md)
