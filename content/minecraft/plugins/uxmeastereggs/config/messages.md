---
title: Messages
order: 605
description: How a line is written, the roles and prefixes it uses, and every block of keys with the values each fills in.
icon: message-square
---

Every line a player reads comes from `messages/messages_<language>.conf`, one file per language. No text is written
in the code, so any line can be rewritten and the plugin translated whole. Run `/uxmeastereggs reload` after an edit.

## How a line is written

```hocon
egg {
  progress = "<tag:'EGGS'> <body>You have found <value><found><body> of <value><total><body> eggs."
}
```

- The file is HOCON blocks: `egg.progress` is the key `progress` inside the block `egg`.
- A line is [MiniMessage](https://docs.advntr.dev/minimessage/format.html). Legacy `&a` codes are shown as written.
- A line names a **role**, not a colour: `<body>` is the text, `<value>` a name or a number, `<good>`, `<bad>` and
  `<warn>` a result. The colours behind the roles are in [theme.conf](theme.md), shared by every UXM plugin.
- A tag that is not a role is a **value** the plugin fills in, such as `<found>`. The values of each line are fixed;
  they are listed below. A value is inserted as text, never as markup.
- Write ordinary letters. Small capitals are applied when the line is drawn, for the languages
  [theme.conf](theme.md#small-capitals) turns them on for, so a translator reads plain text.
- A line you delete falls back to the default built into the plugin, so a missing key is never blank.

## The prefixes

| Tag | Draws |
|---|---|
| `<tag:'EGGS'>` | The category word in small capitals and the separator glyph, in the category's colour |
| `<tag:'MONEY'>` | The same, for a payout, in the `good` colour |
| `<etag:'ERROR'>` | The same for an error, in the `bad` colour |
| `<h:'How this plugin is'>` | A header, in the `header` gradient |

A category's colour is set under `prefix.categories` in [theme.conf](theme.md#prefix); `EGGS` is not named there, so
it reads in the accent colour. The argument in quotes is plain text: never put a value inside it.

## The keys

### Finding

| Key | Values | Said when |
|---|---|---|
| `egg.found` | `<player>`, `<type>`, `<found>`, `<total>` | The shipped type effects: *You found a Golden Egg* |
| `egg.count` | `<found>`, `<total>` | The shipped `common` effects, on the action bar |
| `egg.progress` | `<found>`, `<total>` | The `progress` moment's line |
| `egg.already-found` | | A once-only egg clicked again |
| `egg.cooling` | `<time>` | A repeatable egg clicked inside its cooldown |
| `egg.unknown-type` | | An egg whose type file is gone |
| `type.<type>.name` | | The display name of a type that names it with `@`. Ships `type.common.name`, `type.golden.name` |
| `type.golden.hint` | | The subtitle of the shipped golden effects |
| `milestone.reached` | `<count>` | The `milestone` moment's line |
| `milestone.broadcast` | `<player>`, `<count>` | The `milestone-broadcast` moment's line |
| `completion.reached` | | The `completion` moment's line |
| `completion.broadcast` | `<player>` | The `completion-broadcast` moment's line |
| `radar.on`, `radar.off` | | The `radar-on` and `radar-off` moments' lines |
| `radar.hint` | `<distance>` | The radar, every interval |

### Placing eggs

| Key | Values | Said when |
|---|---|---|
| `admin.placed` | `<type>`, `<x>`, `<y>`, `<z>`, `<id>` | `create` |
| `admin.occupied` | `<id>` | An egg already sits on the block |
| `admin.type-unknown` | `<type>` | A type with no file |
| `admin.updated` | `<id>` | `set` |
| `admin.removed` | `<id>` | `delete` |
| `admin.not-found` | | No egg with that id |
| `admin.no-such-piece` | `<piece>` | `set` with a piece other than `type` or `position` |
| `admin.look-at-block` | | Nothing within six blocks in your line of sight |
| `admin.type-needed`, `admin.egg-needed`, `admin.position-needed`, `admin.world-needed` | | The console left out what a player would look at |
| `admin.invalid-position` | `<written>` | A position that is not three whole numbers or `here` |
| `admin.list-header`, `admin.list-entry`, `admin.list-empty` | `<count>`; `<id>`, `<type>`, `<world>`, `<x>`, `<y>`, `<z>` | `list` |
| `admin.show.header`, `admin.show.line` | `<id>`; `<piece>`, `<setting>` | `show` |
| `admin.teleported`, `admin.world-missing` | `<id>`; `<world>` | `tp` |
| `admin.player-reset`, `admin.player-unknown` | `<player>` | `reset` |

### Boards

| Key | Values | Said when |
|---|---|---|
| `top.header`, `top.line` | `<board>`, `<period>`; `<rank>`, `<player>`, `<score>` | `/eggs top <board>` |
| `top.empty`, `top.unknown` | `<board>`, `<period>` | An empty board, or an unknown board or period |
| `board.found` | | The name of the board |
| `period.alltime`, `period.monthly`, `period.weekly`, `period.daily` | | The names of the periods |
| `placeholder.nobody`, `placeholder.none`, `placeholder.resets.*` | `<days>`, `<hours>`, `<minutes>` | What the board placeholders answer for an empty place, and the time to a reset |

### Windows

| Block | Holds |
|---|---|
| `menu.lore.description`, `menu.lore.details`, `menu.button.previous`, `menu.button.next` | The tooltip headers and the page arrows |
| `menu.missing` | A window whose file is gone, with `<menu>` |
| `menu.collection.*` | The collection's title, the type tile, and `state-complete`, `state-partial`, `state-missing`, `action-complete`, `action-partial`, `action-missing` |
| `menu.summary.*` | The *Your progress* tile |
| `menu.leaderboard.*` | The leaderboard's title, the player tile and the *Nobody yet* tile |
| `menu.admin.*` | The placed eggs window: title, egg tile with its click lines, *No egg yet* |
| `menu.picker.*` | The type picker: title, type tile, *No type yet* |

### Language, commands and the plugin

| Block | Holds |
|---|---|
| `language.*` | The replies of `/eggs lang` and its subcommands |
| `command.*` | The replies every command shares: no permission, player only, a value that is not valid, the help pages |
| `admin.version`, `admin.reloaded`, `admin.reload-failed` | `version` and `reload`, with `<version>` and `<ms>` |
| `admin.doctor.*` | The header, the `ok`, `warn` and `fail` rows with `<check>` and `<said>`, and the closing line |

## Notes

- **Edit every language.** A line rewritten in English stays as it was in the other files; change each one you use.
- **A type of your own** reads its name from `type.<type>.name` only when its `display-name` says `@type.<type>.name`;
  add the key to every language.

Related: [Languages](languages.md), [Type files](type-files.md), [Moments](../hunting/moments.md)
