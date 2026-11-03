---
title: Configuration
order: 150
description: Every file the plugin writes, what belongs in it, reloading, and what an update does to your files.
icon: settings
---

The plugin writes its files into `plugins/uxmLevels/` on first start; the folder is shown in
[Installation](../getting-started/installation.md#the-files). Every file is HOCON. Run `/uxmlevels reload`
(`uxmlevels.admin.reload`) after an edit; the keys that need a restart say so on their page.

| Page | What it holds |
|---|---|
| [config.conf](config-conf.md) | Language, storage, economy, experience, anti-abuse, leaderboard, hud, prestige, window sounds |
| [Track files](track-files.md) | `tracks/<id>.conf`: one track each, with its curve, heads-up surface and rewards |
| [Event files](event-files.md) | `events/<name>.conf`: what pays into which track |
| [commands.conf](commands-conf.md) | The name, aliases and switch of every command |
| [Menu files](menu-files.md) | `menus/`: the four windows |
| [Messages](messages.md) | `messages/messages_<language>.conf`: every line the plugin writes |
| [Languages](languages.md) | The seven languages, adding one, and how a player's language is chosen |
| [theme.conf](theme.md) | The colours, glyphs and small capitals every UXM plugin shares |

## HOCON in two minutes

| Rule | Example |
|---|---|
| A block opens and closes with braces | `prestige { maximum = 10 }` |
| `=` or `:` between key and value; a block needs neither | `growth = 1.15`, `hud { ... }` |
| Text in quotes, numbers and `true`/`false` without | `icon = "IRON_SWORD"`, `boss-bar = true` |
| Lists in square brackets, commas or new lines between | `filter = ["PLAYER"]` |
| A comment starts with `#` | `# the default is 100` |
| A key with dots in it is quoted | `"uxmlevels.multiplier.vip" = 1.25` |

A file that does not parse stops the reload, keeps the configuration that worked, and the console names the file
and the line.

## Reloading

`/uxmlevels reload` reads `config.conf`, every track and event file, the window files, the messages and `theme.conf`
again, restarts the heads-up and playtime timers with the new periods, and swaps the rules in one go. A file that
cannot be read, a duplicate event id or a value out of range stops the reload with `admin.reload-failed`, and the
server keeps the rules that worked.

| Not read by a reload | Why |
|---|---|
| `commands.conf` | The server builds its command tree once, at start |
| `storage` | The database is opened once, at start |
| `economy` | The economies are found once, at start |
| `leaderboard`, except `chat-lines` and `empty-head` | The board timers and periods are made at start |
| Permission multipliers and track nodes a player holds | Read when the player joins |

## What an update does

You never replace your files after an update. On the start after a new jar:

1. `config.conf`, `commands.conf` and every window file the jar ships that is missing is written. The example tracks
   and events are written only into a `tracks/` or `events/` folder that does not exist yet.
2. A key the release added is written into your `config.conf` with the comment that explains it, at its default.
3. A value you set is never changed.
4. Your file as it was goes to `backups/<file>-<date>.conf`; the last five copies of each file are kept.

A `config.conf` from an earlier release with `leaderboard.refresh-ms` has it carried over to `refresh-seconds`.

Related: [config.conf](config-conf.md), [Installation](../getting-started/installation.md)
