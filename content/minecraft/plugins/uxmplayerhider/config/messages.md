---
title: Messages
order: 403
description: How a line is written, the roles and prefixes it uses, and every block of keys with the values each fills in.
icon: message-square
---

Every line the plugin writes comes from `messages/messages_<language>.conf`, one file per language. No text is written
in the code, so any line can be rewritten and the plugin translated whole. Run `/uxmplayerhider reload` after an edit.

## How a line is written

```hocon
error {
  cooldown = "<etag:'ERROR'> <body>Wait <value><seconds><body> more seconds."
}
```

- The file is HOCON blocks: `error.cooldown` is the key `cooldown` inside the block `error`.
- A line is [MiniMessage](https://docs.advntr.dev/minimessage/format.html). Legacy `&a` codes are shown as written.
- A line names a **role**, not a colour: `<body>` is the text, `<value>` a name or a number, `<good>`, `<bad>` and
  `<warn>` a result. The colours behind the roles are in [theme.conf](theme.md), shared by every UXM plugin.
- A tag that is not a role is a **value** the plugin fills in, such as `<seconds>`. The values of each line are
  fixed; they are listed below. A value is inserted as text, never as markup.
- Write ordinary letters. Small capitals are applied when the line is drawn, for the languages
  [theme.conf](theme.md#small-capitals) turns them on for, so a translator reads plain text.
- A line you delete falls back to the default built into the plugin, so a missing key is never blank.

## The prefixes

| Tag | Draws |
|---|---|
| `<tag:'HIDER'>` | The category word in small capitals and the separator glyph, in the category's colour |
| `<etag:'ERROR'>` | The same for an error, in the `bad` colour |
| `<h:'How this plugin is'>` | A header, in the `header` gradient |

A category's colour is set under `prefix.categories` in [theme.conf](theme.md#prefix); `HIDER` is not named there, so
it reads in the accent colour. The argument in quotes is plain text: never put a value inside it.

## The keys

| Key | Values | Said when |
|---|---|---|
| `status` | `<state>`, `<hidden>` | `/hide status`. `<state>` is the state's `name` line |
| `state.<state>.name` | | The state inside a sentence; it carries its own colour |
| `state.<state>.title`, `subtitle`, `action-bar`, `chat` | | Named by the shipped [effects](../hiding/effects.md); `chat` only once you turn its line on |
| `tool.title`, `tool.crumb`, `tool.status-label`, `tool.description` | | The tool's tooltip, the same in every state. `\n` in `description` breaks the line |
| `tool.lore.description`, `tool.lore.details` | | The two section headers in the tooltip |
| `tool.<state>.name`, `tool.<state>.lore` | | The state row and the click line of the tooltip |
| `error.cooldown` | `<seconds>` | A change inside the cooldown |
| `error.world-not-allowed` | | A change in a world not in `worlds` |
| `admin.usage` | | The bare `/uxmplayerhider` |
| `admin.version`, `admin.reloaded`, `admin.reload-failed` | `<version>`, `<ms>` | `version` and `reload` |
| `admin.doctor.*` | `<check>`, `<said>` | `doctor` |
| `language.*` | `<language>`, `<languages>`, `<player>`, `<input>` | The replies of `/hide lang` and its subcommands |
| `command.*` | | The replies every command shares: no permission, player only, a value that is not valid, the help pages |

A state's effects can name any key with `@`, including one you add: add `state.none.bossbar = "<bad>Hidden"` to
every file and write `[bossbar] 3 red | @state.none.bossbar` in `feedback.effects.none`.

## Notes

- **Edit every language.** A line rewritten in English stays as it was in the other files; change each one you use.
- **"Staff and friends".** The shipped `important` lines name staff and friends; the plugin shows whoever holds
  `uxmplayerhider.important`. Reword them to match who you grant it to.

Related: [Languages](languages.md), [theme.conf](theme.md), [Effects](../hiding/effects.md)
