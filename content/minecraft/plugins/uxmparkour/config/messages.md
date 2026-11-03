---
title: Messages
order: 505
description: How a line is written, the roles and prefixes it uses, and every block of keys with the values each fills in.
icon: message-square
---

Every line a player reads comes from `messages/messages_<language>.conf`, one file per language. No text is written
in the code, so any line can be rewritten and the plugin translated whole. Run `/uxmparkour reload` after an edit.

## How a line is written

```hocon
run {
  fell = "<tag:'PARKOUR'> <body>You fell after <value><jumps><body> jumps."
}
```

- The file is HOCON blocks: `run.fell` is the key `fell` inside the block `run`.
- A line is [MiniMessage](https://docs.advntr.dev/minimessage/format.html). Legacy `&a` codes are shown as written.
- A line names a **role**, not a colour: `<body>` is the text, `<value>` a name or a number, `<good>`, `<bad>` and
  `<warn>` a result. The colours behind the roles are in [theme.conf](theme.md), shared by every UXM plugin.
- A tag that is not a role is a **value** the plugin fills in, such as `<jumps>`. The values of each line are fixed;
  they are listed below.
- Write ordinary letters. Small capitals are applied when the line is drawn, for the languages
  [theme.conf](theme.md#small-capitals) turns them on for, so a translator reads plain text.
- A line you delete falls back to the default built into the plugin, so a missing key is never blank.

## The prefixes

| Tag | Draws |
|---|---|
| `<tag:'PARKOUR'>` | The category word in small capitals and the separator glyph, in the category's colour |
| `<etag:'ERROR'>` | The same for an error, in the `bad` colour |
| `<h:'How this plugin is'>` | A header, in the `header` gradient |

A category's colour is set under `prefix.categories` in [theme.conf](theme.md#prefix); one not named there reads in
the accent colour. The argument in quotes is plain text: never put a value inside it.

## The keys

### Runs

| Key | Values | Said when |
|---|---|---|
| `run.joined` | `<arena>` | A run starts |
| `run.already-running` | `<arena>` | Joining while in a run |
| `run.no-such-arena` | `<arena>` | Joining an arena that does not exist or is shut |
| `run.no-arenas` | | Joining with no open arena |
| `run.left` | `<jumps>` | Leaving by command or item |
| `run.not-running` | | Leaving or restarting outside a run |
| `run.fell` | `<jumps>` | The `fell` moment's line |
| `run.restarted` | `<jumps>` | A restart, with the old run's jumps |
| `run.exhausted` | `<jumps>` | The `exhausted` moment's line |
| `run.personal-best` | `<jumps>`, `<best>` | The `personal-best` moment's line |
| `run.summary` | `<best>`, `<jumps>` | After a fall or exhaustion that is not a personal best |

### Milestones

| Key | Values | Said when |
|---|---|---|
| `milestone.reached` | `<jumps>`, `<reward>` | The `milestone` moment's line |
| `milestone.no-economy` | `<jumps>` | A milestone line could not pay |
| `milestone.broadcast` | `<player>`, `<jumps>` | The `broadcast` moment's line, to everyone |

### Records and boards

| Key | Values | Said when |
|---|---|---|
| `stats.header`, `stats.best`, `stats.total-jumps`, `stats.total-runs` | `<player>`, `<best>`, `<jumps>`, `<runs>` | `/parkour stats` |
| `stats.reset` | `<player>` | `/uxmparkour reset` erased a runner |
| `stats.unknown` | `<player>` | Nobody by that name has run |
| `top.header`, `top.line` | `<board>`, `<period>`; `<rank>`, `<player>`, `<score>` | `/parkour top` |
| `top.empty`, `top.unknown` | `<board>`, `<period>` | An empty board, an unknown board or period |
| `board.best`, `board.jumps`, `board.runs` | | The names of the boards in `top.header` |
| `period.alltime`, `period.monthly`, `period.weekly`, `period.daily` | | The names of the periods |
| `placeholder.nobody`, `placeholder.none` | | What a board placeholder answers for a place nobody holds |
| `placeholder.resets.days`, `.hours`, `.minutes` | `<days>`, `<hours>`, `<minutes>` | How `%uxmparkour_resets_<period>%` writes the time left |

### Choices

| Key | Values | Said when |
|---|---|---|
| `theme.chosen`, `theme.unknown` | `<theme>` | `/parkour theme`, and the picker |
| `difficulty.chosen`, `difficulty.unknown` | `<difficulty>` | `/parkour difficulty` |

### Arenas

| Key | Values | Said when |
|---|---|---|
| `arena.created` | `<arena>` | `create` |
| `arena.edit-updated` | `<arena>`, `<missing>` | A piece set, others still missing |
| `arena.ready`, `arena.saved` | `<arena>` | Every piece set, on a shut or an open arena |
| `arena.exists`, `arena.rejected` | `<arena>`, `<reason>` | A name in use, or a value an arena cannot take |
| `arena.opened`, `arena.shut`, `arena.deleted` | `<arena>` | `enable`, `disable`, `delete` |
| `arena.shut-incomplete`, `arena.shut-fault` | `<arena>`, `<missing>` | An edit shut an open arena |
| `arena.incomplete` | `<missing>` | `enable` with pieces missing |
| `arena.fault.start-outside-boundary` | | The start block is outside the box |
| `arena.teleported`, `arena.no-start`, `arena.world-missing` | `<arena>`, `<world>` | `tp` |
| `arena.world-needed`, `arena.here-needs-a-player`, `arena.no-such-piece` | `<piece>` | Console mistakes with `create` and `set` |
| `arena.list`, `arena.list-empty` | `<arenas>` | `list` from the console |
| `arena.show.header`, `.line`, `.unset` | `<arena>`, `<piece>`, `<setting>` | `show` |
| `arena.mark.prompt`, `.done`, `.cancelled` | `<piece>` | Marking a block from the editor |
| `arena.name.prompt` | | The anvil asking for a new arena's name |

### Windows and items

| Block | Holds |
|---|---|
| `menu.title`, `menu.button.previous`, `menu.button.next` | The theme picker's title and the page arrows |
| `menu.lore.description`, `menu.lore.details` | The two headers of every tooltip |
| `menu.theme.*` | The theme picker's tooltip: crumb, description, state, click line |
| `menu.admin.*` | The arena list and editor: titles, tooltips, the six pieces, the switch, **Back** |
| `menu.missing` | A window file that does not exist |
| `item.restart.name`, `item.restart.lore`, `item.leave.name`, `item.leave.lore` | The session items |

### Language, commands and the plugin

| Block | Holds |
|---|---|
| `language.*` | The replies of `/parkour lang` and its subcommands |
| `command.*` | The replies every command shares: no permission, player only, a value that is not valid, the help pages |
| `admin.version`, `admin.reloaded`, `admin.reload-failed` | `version` and `reload`, with `<version>` and `<ms>` |
| `admin.doctor.*` | The header, the `ok`, `warn` and `fail` rows with `<check>` and `<said>`, and the closing line |

## Notes

- **Edit every language.** A line rewritten in English stays as it was in the other files; change each one you use.
- **Values in a moment.** A [moment](../arenas/moments.md) line names one of these keys with `@`, and its values are
  filled in the same way.
- **A key a release adds** reaches your files in your own language, with the English line as its fallback.

Related: [Languages](languages.md), [theme.conf](theme.md), [Moments](../arenas/moments.md)
