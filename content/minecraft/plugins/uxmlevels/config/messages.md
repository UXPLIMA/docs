---
title: Messages
order: 606
description: How a line is written, the roles and prefixes it uses, and every block of keys with the values each fills in.
icon: message-square
---

Every line the plugin writes comes from `messages/messages_<language>.conf`, one file per language. No text is written
in the code, so any line can be rewritten and the plugin translated whole. Run `/uxmlevels reload` after an edit.

## How a line is written

```hocon
levels {
  level-up = "<tag:'LEVELS'> <body>You are now level <value><level><body> in <track><body>."
}
```

- The file is HOCON blocks: `levels.level-up` is the key `level-up` inside the block `levels`.
- A line is [MiniMessage](https://docs.advntr.dev/minimessage/format.html). Legacy `&a` codes are shown as written.
- A line names a **role**, not a colour: `<body>` is the text, `<value>` a name or a number, `<good>`, `<bad>` and
  `<warn>` a result. The colours behind the roles are in [theme.conf](theme.md).
- A tag that is not a role is a **value** the plugin fills in, such as `<level>`. A value is inserted as text.
- `<track>` is the track's `display-name`, in its own colour.
- Write ordinary letters. Small capitals are applied when the line is drawn, for the languages
  [theme.conf](theme.md#small-capitals) turns them on for.
- A line you delete falls back to the default built into the plugin.

## The prefixes

| Tag | Draws |
|---|---|
| `<tag:'LEVELS'>` | The category word and the separator glyph, in the category's colour |
| `<etag:'ERROR'>` | The same for an error, in the `bad` colour |
| `<h:'...'>` | A header, in the `header` gradient |

## The keys

| Key | Values | Said when |
|---|---|---|
| `levels.gain-action-bar` | `<amount>`, `<track>` | A gain from an event with `action-bar = true` |
| `levels.level-up` | `<level>`, `<track>` | A level up |
| `levels.hourly-cap-reached` | `<track>` | An earning held back by the hourly cap |
| `levels.own-level` | `<track>`, `<level>`, `<maximum>`, `<into>`, `<cost>` | `/levels level` |
| `levels.other-level` | `<player>`, `<track>`, `<level>`, `<maximum>` | `/levels level of` |
| `levels.maximum-reached` | `<track>` | `/levels level` on a finished track |
| `levels.no-tracks` | | A server with no track |
| `reward.claimed`, `already-claimed`, `not-claimable`, `granted-automatically` | `<reward>` | Claiming, and an automatic reward |
| `reward.not-reached` | `<level>`, `<held>` | A reward above the player's level |
| `prestige.done` | `<track>`, `<prestige>`, `<multiplier>` | A prestige |
| `prestige.level-too-low` | `<level>`, `<held>` | |
| `prestige.maximum` | | |
| `prestige.not-enough-experience` | `<required>`, `<experience>` | |
| `prestige.no-economy`, `prestige.cannot-afford` | `<cost>` | A money cost that cannot be paid |
| `boost.started` | `<player>`, `<multiplier>`, `<duration>` | `boost` |
| `boost.stopped`, `boost.nothing-running` | `<player>` | `unboost` |
| `top.header`, `top.line`, `top.empty`, `top.unranked`, `top.unknown` | `<track>`, `<position>`, `<player>`, `<level>`, `<experience>`, `<limit>`, `<period>` | `/levels top` |
| `top.period-header`, `top.period-line` | the same | `/levels top <track> <period>` |
| `period.alltime`, `monthly`, `weekly`, `daily` | | The name of a period |
| `placeholder.nobody`, `placeholder.none` | | An empty place on a board |
| `placeholder.resets.days`, `hours`, `minutes` | `<days>`, `<hours>`, `<minutes>` | `%uxmlevels_resets_<period>%` |
| `menu.*` | | The four windows |
| `hud.boss-bar`, `hud.action-bar` | `<track>`, `<level>`, `<percent>`, `<into>`, `<cost>` | The heads-up surfaces |
| `track.<track>.name`, `track.<track>.<reward>.name`, `.lore` | | The shipped tracks and rewards, named with `@` |
| `admin.*` | `<amount>`, `<player>`, `<track>`, `<level>`, `<maximum>`, `<version>`, `<ms>` | The operator replies |
| `error.*` | `<track>`, `<player>`, `<reward>`, `<duration>` | Unknown names and refused values |
| `language.*` | `<language>`, `<languages>`, `<player>`, `<input>` | `/levels lang` |
| `command.*` | | The replies every command shares |

A track or reward you add names its words with `@`: write `display-name = "@track.farming.name"` and add
`track.farming.name` to every file.

Related: [Languages](languages.md), [theme.conf](theme.md)
