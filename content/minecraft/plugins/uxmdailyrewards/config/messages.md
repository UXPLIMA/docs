---
title: Messages
order: 604
description: How a line is written, the roles and prefixes it uses, and every block of keys with the values each fills in.
icon: message-square
---

Every line the plugin writes comes from `messages/messages_<language>.conf`, one file per language. No text is written
in the code, so any line can be rewritten and the plugin translated whole. Run `/uxmdailyrewards reload` after an edit.

## How a line is written

```hocon
claim {
  claimed = "<tag:'DAILY'> <body>You claimed day <value><day><body>. Your streak is <value><streak><body>."
}
```

- The file is HOCON blocks: `claim.claimed` is the key `claimed` inside the block `claim`.
- A line is [MiniMessage](https://docs.advntr.dev/minimessage/format.html). Legacy `&a` codes are shown as written.
- A line names a **role**, not a colour: `<body>` is the text, `<value>` a name or a number, `<good>`, `<bad>` and
  `<warn>` a result. The colours behind the roles are in [theme.conf](theme.md), shared by every UXM plugin.
- A tag that is not a role is a **value** the plugin fills in, such as `<day>`. The values of each line are fixed;
  they are listed below. A value is inserted as text, never as markup.
- Write ordinary letters. Small capitals are applied when the line is drawn, for the languages
  [theme.conf](theme.md#small-capitals) turns them on for, so a translator reads plain text.
- A line you delete falls back to the default built into the plugin, so a missing key is never blank.

## The prefixes

| Tag | Draws |
|---|---|
| `<tag:'DAILY'>` | The category word in small capitals and the separator glyph, in the category's colour |
| `<etag:'ERROR'>` | The same for an error, in the `bad` colour |
| `<h:'How this plugin is'>` | A header, in the `header` gradient |

A category's colour is set under `prefix.categories` in [theme.conf](theme.md#prefix); `DAILY` is not named there, so
it reads in the accent colour.

## The keys

| Key | Values | Said when |
|---|---|---|
| `claim.claimed` | `<day>`, `<streak>` | A claim went through |
| `claim.streak-reset` | `<day>` | Before `claimed`, when the streak ran out |
| `claim.too-soon` | `<time>`, `<days>`, `<hours>`, `<minutes>`, `<seconds>` | A claim inside the window |
| `claim.not-stored` | | The database refused the claim |
| `claim.no-such-day` | `<day>` | The day has no file |
| `claim.in-progress` | | A second claim while the first is written |
| `reminder.title`, `subtitle`, `action-bar` | `<day>`, `<streak>` | Named by the shipped reminder |
| `menu.title` | | The window title |
| `menu.missing` | `<menu>` | The window file is missing |
| `menu.unavailable` | | The calendar cannot be read |
| `menu.button.previous`, `menu.button.next` | | The page arrows |
| `menu.lore.description`, `menu.lore.details` | | The two section headers of every tooltip |
| `menu.day.title` | `<day>` | A day's title |
| `menu.day.crumb`, `menu.day.description` | | A day's crumb and description |
| `menu.day.state-label`, `menu.day.state-<state>` | | The state row |
| `menu.day.action-<state>` | | The click line |
| `menu.day.reward.label`, `separator` | | The reward row and what joins its parts |
| `menu.day.reward.item`, `coins`, `experience`, `levels`, `points`, `currency` | `<amount>`, `<item>`, `<currency>` | One part of the reward row |
| `menu.day.rewards.<day>` | | A day's own reward line, when you write one |
| `menu.info.*` | `<time>` on `state-waiting` | The streak tile |
| `admin.reset-done`, `admin.reset-nothing` | `<player>` | `reset` |
| `admin.granted` | `<day>`, `<player>` | `grant` |
| `admin.grant-no-such-day` | `<day>`, `<length>` | `grant` with a day outside the calendar |
| `admin.player-offline` | `<player>` | `reset` with a name the server has never seen |
| `admin.usage`, `admin.version`, `admin.reloaded`, `admin.reload-failed` | `<version>`, `<ms>` | The bare root, `version` and `reload` |
| `admin.doctor.*` | `<check>`, `<said>` | `doctor` |
| `language.*` | `<language>`, `<languages>`, `<player>`, `<input>` | The replies of `/dailyrewards lang` and its subcommands |
| `command.*` | | The replies every command shares: no permission, player only, a value that is not valid, the help pages |

`<state>` is `claimed`, `claimable`, `waiting` or `locked`. A day's lines can name any key with `@`, including one you
add, with `<day>` and `<streak>` filled in.

## Notes

- **Edit every language.** A line rewritten in English stays as it was in the other files; change each one you use.
- **Item names follow the client.** The item in a reward row is drawn in the viewer's own client language, not from
  these files.

Related: [Languages](languages.md), [theme.conf](theme.md), [Calendar window](../menus/calendar.md)
