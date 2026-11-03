---
title: Messages
order: 505
description: How a line is written, the roles and prefixes it uses, and every block of keys with the values each fills in.
icon: message-square
---

Every line a player reads comes from `messages/messages_<language>.conf`, one file per language. No text is written
in the code, so any line can be rewritten and the plugin translated whole. Run `/uxmkotl reload` after an edit.

## How a line is written

```hocon
crown {
  announce = "<tag:'KOTL'> <body><value><king><body> took the crown of <value><arena><body>."
}
```

- The file is HOCON blocks: `crown.announce` is the key `announce` inside the block `crown`.
- A line is [MiniMessage](https://docs.advntr.dev/minimessage/format.html). Legacy `&a` codes are shown as written.
- A line names a **role**, not a colour: `<body>` is the text, `<value>` a name or a number, `<good>`, `<bad>` and
  `<warn>` a result. The colours behind the roles are in [theme.conf](theme.md), shared by every UXM plugin.
- A tag that is not a role is a **value** the plugin fills in, such as `<king>`. The values of each line are fixed;
  they are listed below. A value is inserted as text, never as markup.
- Write ordinary letters. Small capitals are applied when the line is drawn, for the languages
  [theme.conf](theme.md#small-capitals) turns them on for, so a translator reads plain text.
- A line you delete falls back to the default built into the plugin, so a missing key is never blank.

## The prefixes

| Tag | Draws |
|---|---|
| `<tag:'KOTL'>` | The category word in small capitals and the separator glyph, in the category's colour |
| `<tag:'MONEY'>` | The same, for a payout, in the `good` colour |
| `<etag:'ERROR'>` | The same for an error, in the `bad` colour |
| `<h:'How this plugin is'>` | A header, in the `header` gradient |

A category's colour is set under `prefix.categories` in [theme.conf](theme.md#prefix); `KOTL` is not named there, so
it reads in the accent colour. The argument in quotes is plain text: never put a value inside it.

## The keys

### The crown and fights

| Key | Values | Said when |
|---|---|---|
| `crown.claimed` | `<arena>`, `<king>` | The `crowned` moment's line, to the new king |
| `crown.announce` | `<arena>`, `<king>` | The `crowned` moment's broadcast, to the arena |
| `crown.lost` | `<time>` | The `dethroned` moment's line |
| `crown.blocked` | `<time>` | The `claim-blocked` moment's line |
| `kill.credited` | `<player>` | The `knocked-off` moment's line, to the killer |
| `kill.died` | | The `fell` moment's line |
| `reward.paid` | `<amount>` | The `paid` moment's line |
| `stick.name`, `stick.lore` | | The knockback stick's name and lore, in each viewer's language |

### Joining and leaving

| Key | Values | Said when |
|---|---|---|
| `arena.joined` | `<arena>` | The `joined` moment's line |
| `arena.left` | `<arena>` | The `left` moment's line |
| `arena.list`, `arena.list-empty` | `<arenas>` | The bare `/kotl` |
| `error.unknown-arena` | `<arena>` | No arena by that id, or one still missing a piece |
| `error.arena-disabled` | `<arena>` | Joining an arena that is off |
| `error.already-playing` | `<arena>` | Joining while in an arena |
| `error.not-playing` | | Leaving outside an arena |
| `error.player-only` | | A player command from the console |

### Records and boards

| Key | Values | Said when |
|---|---|---|
| `stats.summary` | `<total>`, `<longest>`, `<claims>`, `<kills>` | `/kotl stats` |
| `stats.reset` | `<player>` | `/uxmkotl reset` erased a player |
| `stats.unknown` | `<player>` | Nobody by that name has played |
| `top.header`, `top.line` | `<board>`, `<period>`; `<rank>`, `<player>`, `<score>` | `/kotl top <board>` |
| `top.empty` | | A board with nobody on it |
| `top.unknown` | `<board>`, `<period>` | An unknown board or period |
| `board.held`, `board.longest`, `board.claims`, `board.kills` | | The names of the boards in `top.header` |
| `period.alltime`, `period.monthly`, `period.weekly`, `period.daily` | | The names of the periods |
| `placeholder.nobody`, `placeholder.none` | | What a placeholder answers for a free plate or a place nobody holds |
| `placeholder.resets.days`, `.hours`, `.minutes` | `<days>`, `<hours>`, `<minutes>` | How `%uxmkotl_resets_<period>%` writes the time left |

### Building arenas

| Key | Values | Said when |
|---|---|---|
| `arena.created` | `<arena>` | `create` |
| `arena.updated` | `<arena>`, `<missing>` | A piece set, others still missing |
| `arena.ready`, `arena.saved` | `<arena>` | Every piece set, on a shut or an open arena |
| `arena.incomplete` | `<missing>` | `enable`, or `tp`, with pieces missing |
| `arena.shut-incomplete`, `arena.shut-fault` | `<arena>`, `<missing>` | An edit switched an open arena off |
| `arena.fault.plate-outside-boundary` | | The plate is outside the box |
| `arena.enabled`, `arena.turned-off`, `arena.deleted` | `<arena>` | `enable`, `disable`, `delete` |
| `arena.teleported` | `<arena>` | `tp` |
| `arena.list-header`, `arena.list-on`, `arena.list-off` | `<count>`; `<arena>` | `/uxmkotl list` in chat |
| `arena.world-needed`, `arena.here-needs-a-player` | | `create` or `here` from the console |
| `arena.no-such-piece`, `arena.not-a-switch`, `arena.no-such-mode` | `<piece>`, `<written>` | A `set` that names nothing |
| `arena.show.header`, `.line`, `.unset` | `<arena>`, `<piece>`, `<setting>` | `show` |
| `arena.mark.prompt`, `.done`, `.cancelled` | `<piece>` | Marking a block from the editor |
| `arena.name.prompt` | | The anvil asking for a new arena's name |
| `error.invalid-id`, `error.arena-exists` | `<arena>` | A name that is not an id, or one in use |
| `error.invalid-position` | `<written>` | A position that is not three whole numbers or `here` |
| `error.arena-in-use` | `<arena>`, `<players>` | `delete` with players inside |
| `error.world-missing` | `<world>` | `tp` to a world that is not loaded |

### Windows

| Block | Holds |
|---|---|
| `menu.lore.description`, `menu.lore.details` | The two headers of every tooltip |
| `menu.button.previous`, `menu.button.next` | The page arrows |
| `menu.value.on`, `.off`, `.unset`, `.incomplete` | The words a window writes into a line of your own |
| `menu.missing` | A window that is not on the server, with `<menu>` |
| `menu.editor.*` | The editor's title (`<arena>`), `gone`, and one block per button, with a block per state for the switch and the pieces |
| `menu.list.*` | The list's title, the arena tile per state (`on`, `off`, `incomplete`) and **New arena** |
| `menu.stats.*` | The record window's title and its tile, one block per fact |
| `menu.top.*` | The leaderboard's title, the player tile and the *Nobody yet* tile |

### Language, commands and the plugin

| Block | Holds |
|---|---|
| `language.*` | The replies of `/kotl lang` and its subcommands |
| `command.*` | The replies every command shares: no permission, player only, a value that is not valid, a cooldown, the help pages |
| `admin.version`, `admin.reloaded`, `admin.reload-failed` | `version` and `reload`, with `<version>` and `<ms>` |
| `admin.doctor.*` | The header, the `ok`, `warn` and `fail` rows with `<check>` and `<said>`, and the closing line |

## Notes

- **Edit every language.** A line rewritten in English stays as it was in the other files; change each one you use.
- **Values in a moment.** A [moment](../arenas/moments.md) line names one of these keys with `@`, and its values are
  filled in the same way.
- **`arena.mark.prompt`** says *Type cancel to stop*, but a swing at the air is what cancels. Rewrite the line in
  your files if your players read it.

Related: [Languages](languages.md), [theme.conf](theme.md), [Moments](../arenas/moments.md)
