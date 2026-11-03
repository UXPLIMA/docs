---
title: Messages
order: 504
description: How a line is written, the roles and prefixes it uses, and every block of keys with the values each fills in.
icon: message-square
---

Every line a player reads comes from `messages/messages_<language>.conf`, one file per language. No text is written
in the code, so any line can be rewritten and the plugin translated whole. Run `/uxmhubpvp reload` after an edit.

## How a line is written

```hocon
reward {
  kill = "<tag:'PVP'> <body>You beat <value><victim><body>. Streak: <value><streak>"
}
```

- The file is HOCON blocks: `reward.kill` is the key `kill` inside the block `reward`.
- A line is [MiniMessage](https://docs.advntr.dev/minimessage/format.html). Legacy `&a` codes are shown as written.
- A line names a **role**, not a colour: `<body>` is the text, `<value>` a name or a number, `<good>`, `<bad>` and
  `<warn>` a result. The colours behind the roles are in [theme.conf](theme.md), shared by every UXM plugin.
- A tag that is not a role is a **value** the plugin fills in, such as `<victim>`. The values of each line are fixed;
  they are listed below. A value is inserted as text, never as markup.
- Write ordinary letters. Small capitals are applied when the line is drawn, for the languages
  [theme.conf](theme.md#small-capitals) turns them on for, so a translator reads plain text.
- A line you delete falls back to the default built into the plugin, so a missing key is never blank.

## The prefixes

| Tag | Draws |
|---|---|
| `<tag:'PVP'>` | The category word in small capitals and the separator glyph, in the category's colour |
| `<tag:'MONEY'>` | The same, for a payout, in the `good` colour |
| `<etag:'ERROR'>` | The same for an error, in the `bad` colour |
| `<h:'How this plugin is'>` | A header, in the `header` gradient |

A category's colour is set under `prefix.categories` in [theme.conf](theme.md#prefix); `PVP` is not named there, so
it reads in the accent colour. The argument in quotes is plain text: never put a value inside it.

## The keys

### Fighting

| Key | Values | Said when |
|---|---|---|
| `pvp.warm-up-enter`, `pvp.warm-up-leave` | `<seconds>` | The warm-up moments' lines |
| `pvp.warm-up-bar-enter`, `pvp.warm-up-bar-leave` | `<time>` | The action bar countdown, every second |
| `pvp.warm-up-cancelled`, `pvp.warm-up-moved` | | A warm-up stopped |
| `pvp.on`, `pvp.off` | | The `pvp-on` and `pvp-off` moments' lines |
| `pvp.store-failed` | | The inventory could not be saved, so PvP stayed off |
| `pvp.recovered` | | Items given back on join after a crash |
| `pvp.you-not-in` | | A player who is not fighting hit a fighter |
| `pvp.target-not-in` | `<name>` | A fighter hit a player who is not fighting |
| `combat.tagged` | `<seconds>` | The `combat-tagged` moment's line |
| `combat.command-blocked` | `<seconds>` | A tagged player typed a command not on the list |
| `reward.kill` | `<victim>`, `<streak>` | The `kill` moment's line |
| `reward.kill-coins` | `<coins>` | The `kill-paid` moment's line |
| `reward.death` | `<killer>` | The `death` moment's line |

### Items

| Key | Holds |
|---|---|
| `item.description` | The *Description* header of the toggle item's tooltip |
| `item.join.title`, `.crumb`, `.line-1`, `.line-2`, `.action` | The lobby face of the toggle item |
| `item.leave.title`, `.crumb`, `.line-1`, `.line-2`, `.action` | The leave face |
| `item.blade` | The name of the shipped kit sword |

An item line is drawn as written, spaces and glyphs included, so the toggle item's tooltip lines carry their own
indent and `<icon>` glyph.

### Records and boards

| Key | Values | Said when |
|---|---|---|
| `stats.header` | `<name>` | `/pvp stats` |
| `stats.body` | `<kills>`, `<deaths>`, `<ratio>` | `/pvp stats` |
| `stats.streak` | `<streak>`, `<best>` | `/pvp stats` |
| `stats.reset`, `stats.unknown` | `<player>` | `/uxmhubpvp reset` |
| `error.unknown-player` | `<name>` | `/pvp stats` for a name the server has never seen |
| `top.header`, `top.line` | `<board>`, `<period>`; `<rank>`, `<player>`, `<score>` | `/pvp top` |
| `top.empty` | | A board with nobody on it |
| `top.unknown` | `<board>`, `<period>` | An unknown board or period |
| `board.kills`, `board.streak` | | The names of the boards in `top.header` |
| `period.alltime`, `period.monthly`, `period.weekly`, `period.daily` | | The names of the periods |
| `placeholder.nobody`, `placeholder.none` | | What a board placeholder answers for a place nobody holds |
| `placeholder.resets.days`, `.hours`, `.minutes` | `<days>`, `<hours>`, `<minutes>` | How `%uxmhubpvp_resets_<period>%` writes the time left |

### Language, commands and the plugin

| Block | Holds |
|---|---|
| `language.*` | The replies of `/pvp lang` and its subcommands |
| `command.*` | The replies every command shares: no permission, player only, a value that is not valid, a cooldown, the help pages |
| `admin.usage` | The bare `/uxmhubpvp` |
| `admin.version`, `admin.reloaded`, `admin.reload-failed` | `version` and `reload`, with `<version>` and `<ms>` |
| `admin.doctor.*` | The header, the `ok`, `warn` and `fail` rows with `<check>` and `<said>`, and the closing line |

## Notes

- **Edit every language.** A line rewritten in English stays as it was in the other files; change each one you use.
- **Values in a moment.** A [moment](../fighting/moments.md) line names one of these keys with `@`, and its values
  are filled in the same way.

Related: [Languages](languages.md), [theme.conf](theme.md), [Moments](../fighting/moments.md)
