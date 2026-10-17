---
title: Languages and menus
order: 55
description: The eight languages, the message file and the 13 menu files.
icon: languages
---

Everything players read lives under `languages/<code>/`: chat messages in `messages.yml` and every
menu in `menus/`. Eight languages ship:

| Code | Language |
|---|---|
| `en` | English |
| `tr` | Turkish |
| `es` | Spanish |
| `pt_br` | Brazilian Portuguese |
| `de` | German |
| `fr` | French |
| `ru` | Russian |
| `pl` | Polish |

Set `language` in [config.yml](config-yml.md) and run `/sr reload`. Each server of a network can use
its own language. Event names, rewards and announcements are written in the
[event files](event-files.md), not here.

## messages.yml

Every chat message, plus a few formats used across the plugin:

| Key | What it sets |
|---|---|
| `prefix` | The `<prefix>` at the start of messages |
| `date-format` | How dates appear, `dd.MM.yyyy HH:mm:ss` by default |
| `format.rank-1`, `rank-2`, `rank-3`, `rank` | How ranks are written in menus and placeholders |
| `format.winner-separator` | What separates tied players in announcements |

Text uses MiniMessage: `<#FFD166>` for a colour, `<bold>` or `<b>` for bold, and so on.

## Menus

| File | Menu |
|---|---|
| `tournament-main-menu.yml` | `/sr`: the running tournament, leaderboard, history |
| `tournament-event-menu.yml` | The items players deposit |
| `tournament-amount-menu.yml` | Choosing how many to deposit |
| `tournament-leaderboard-menu.yml` | The current standings |
| `leaderboard-chooser-menu.yml` | Picking which leaderboard to view |
| `history-hub-menu.yml` | Server History and My History |
| `player-history-menu.yml` | A player's past runs |
| `run-summary-menu.yml` | One finished run |
| `run-details-menu.yml` | A player's deposits in one run |
| `event-list-menu.yml` | `/sr events`: every event, and creating one |
| `tournament-manage-menu.yml` | `/sr manage`: one event's settings and items |
| `log-run-picker-menu.yml` | `/sr logs`: choosing a run |
| `event-logs-menu.yml` | The logs of one run |

Each button has a `material`, `display-name`, `lore`, `slots` and `actions`. You can move buttons,
change their look and text, and add `[sound]` or `[player_message]` actions. Keep the button names
(the keys under `items:`) as they are: the plugin looks them up by name.

If a menu file is broken, that menu does not open and the console names the file. There is no
built-in fallback, so check the console after editing a menu.

Related: [config.yml](config-yml.md), [Event files](event-files.md)
