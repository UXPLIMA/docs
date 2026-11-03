---
title: Messages
order: 503
description: How a line is written, the roles and prefixes it uses, and every block of keys with the values each fills in.
icon: message-square
---

Every line the plugin writes comes from `messages/messages_<language>.conf`, one file per language. No text is written
in the code, so any line can be rewritten and the plugin translated whole. Run `/uxmteleportbow reload` after an edit.

## How a line is written

```hocon
give {
  given = "<tag:'BOW'> <body>The teleport bow is yours."
}
```

- The file is HOCON blocks: `give.given` is the key `given` inside the block `give`.
- A line is [MiniMessage](https://docs.advntr.dev/minimessage/format.html). Legacy `&a` codes are shown as written.
- A line names a **role**, not a colour: `<body>` is the text, `<value>` a name or a number, `<good>`, `<bad>` and
  `<warn>` a result. The colours behind the roles are in [theme.conf](theme.md), shared by every UXM plugin.
- A tag that is not a role is a **value** the plugin fills in, such as `<player>`. The values of each line are fixed;
  they are listed below. A value is inserted as text, never as markup.
- Write ordinary letters. Small capitals are applied when the line is drawn, for the languages
  [theme.conf](theme.md#small-capitals) turns them on for, so a translator reads plain text.
- A line you delete falls back to the default built into the plugin, so a missing key is never blank.

## The prefixes

| Tag | Draws |
|---|---|
| `<tag:'BOW'>` | The category word in small capitals and the separator glyph, in the category's colour |
| `<etag:'ERROR'>` | The same for an error, in the `bad` colour |
| `<h:'How this plugin is'>` | A header, in the `header` gradient |

A category's colour is set under `prefix.categories` in [theme.conf](theme.md#prefix); `BOW` is not named there, so
it reads in the accent colour. The argument in quotes is plain text: never put a value inside it.

## The keys

| Key | Values | Said when |
|---|---|---|
| `item.bow-name` | | The bow's name. Ships as a single space, so the bow shows no name line and the lore carries the title |
| `item.bow-lore-1` to `item.bow-lore-9` | | The bow's lore, one key per line; `item.bow.lore-lines` in `config.conf` says how many are shown |
| `give.given` | | `give` with no player: the bow is yours |
| `give.given-to` | `<player>` | `give <player>` |
| `refused.cooldown` | `<seconds>` | A shot inside the cooldown, through the `refused-cooldown` effects |
| `refused.world` | `<world>` | A shot in a world the filter denies, through `refused-world` |
| `refused.departure` | | The region left is closed to the bow, through `refused-departure` |
| `refused.destination` | | The region landed in is closed to the bow, through `refused-destination` |
| `refused.void` | | The arrow stopped below `void-floor-y`, through `refused-void` |
| `refused.headroom` | | No room to stand where the arrow stopped, through `refused-headroom` |
| `error.player-only` | | `give` with no player, from the console |
| `admin.usage` | | The bare `/uxmteleportbow` |
| `admin.version`, `admin.reloaded`, `admin.reload-failed` | `<version>`, `<ms>` | `version` and `reload` |
| `admin.doctor.*` | `<check>`, `<said>` | `doctor` |
| `language.*` | `<language>`, `<languages>`, `<player>`, `<input>` | The replies of `/uxmteleportbow lang` and its subcommands |
| `command.*` | | The replies every command shares: no permission, player only, a value that is not valid, the help pages |

The `refused.*` lines are said only because the shipped effects name them with `@`: a refusal says nothing unless
its list in `config.conf` does. Any effects list can name any key with `@`, including one you add, so a moment can say
something in every language: add `bow.landed = "<accent>Whoosh!"` to every file and write `[actionbar] @bow.landed`
in `effects.arrival`. The values `<seconds>` and `<world>` are filled in `@` lines only, not in your own text.

## Notes

- **Edit every language.** A line rewritten in English stays as it was in the other files; change each one you use.
- **The bow follows the reader.** Its name and lore are drawn for each player in their language on the way to them,
  so two players holding the same bow read it in their own words, and a player who changes language reads the bow
  in the new one within a second. Nothing is rebuilt.
- **A lore line beyond the file.** Raise `item.bow.lore-lines` to 10 and add `bow-lore-10` to every file; a line a
  file lacks falls back as any key does.

Related: [Languages](languages.md), [theme.conf](theme.md), [Effects](../bow/effects.md)
