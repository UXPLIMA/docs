---
title: Configuration
order: 150
description: Every file the plugin writes, what belongs in it, reloading, and what an update does to your files.
icon: settings
---

The plugin writes its files into `plugins/uxmDailyRewards/` on first start; the folder is shown in
[Installation](../getting-started/installation.md#the-files). Every file is HOCON. Run `/uxmdailyrewards reload`
(`uxmdailyrewards.admin.reload`) after an edit; the few keys that need a restart say so on their page.

| Page | What it holds |
|---|---|
| [config.conf](config-conf.md) | Language, storage, the claim window, the calendar length, ranks, reminder, celebration, economy, window sounds |
| [Day files](../rewards/day-files.md) | `days/<number>.conf`: the icon and the reward of each day |
| [Menu files](menu-files.md) | `menus/calendar.conf`: the window |
| [commands.conf](commands-conf.md) | The name, aliases and switch of every command |
| [Messages](messages.md) | `messages/messages_<language>.conf`: every line the plugin writes |
| [Languages](languages.md) | The seven languages, adding one, and how a player's language is chosen |
| [theme.conf](theme.md) | The colours, glyphs and small capitals every UXM plugin shares |

## HOCON in two minutes

| Rule | Example |
|---|---|
| A block opens and closes with braces | `claim { window = "24h" }` |
| `=` or `:` between key and value; a block needs neither | `length = 28`, `claim { ... }` |
| Text in quotes, numbers and `true`/`false` without | `window = "24h"`, `enabled = true` |
| Lists in square brackets, commas or new lines between | `ladder = ["default", "vip", "mvp"]` |
| A comment starts with `#` | `# the default is 28` |
| Two blocks of the same name merge | a `claim { }` at the end adds to the one at the top |

A file that does not parse stops the reload, keeps the configuration that worked, and the console names the file
and the line. It is almost always a missing `}` or quote.

## Reloading

`/uxmdailyrewards reload` reads `config.conf`, every day file, `menus/calendar.conf`, the messages and `theme.conf`
again, and swaps the configuration in one go. A day file or a reward line that cannot be read costs that day or that
line, named in the console; the rest loads. A `config.conf` that does not parse stops the reload with
`admin.reload-failed`, and the server keeps the configuration that worked. The reply says how many milliseconds the
reload took.

| Not read by a reload | Why |
|---|---|
| `commands.conf` | The server builds its command tree once, at start |
| `storage` | The database is opened once, at start |
| `ranks.source` | LuckPerms or permissions is chosen once, at start; the ladder and the prefix do reload |
| `economy.treasury-wait-ms` | The economies are found once, at start |

## What an update does

You never replace your files after an update. On the start after a new jar:

1. Every file the jar ships that is missing is written, the day files and the window included.
2. A key the release added is written into your `config.conf` with the comment that explains it, at its default.
3. A value you set is never changed.
4. Your file as it was goes to `backups/<file>-<date>.conf`; the last five copies of each file are kept.

The file is written again from what it holds, so your own formatting, such as alignment inside a block, may move. A
key you deleted comes back with its default, which is why every switch says in its comment what turning it off
means. A day file you changed is yours and is never rewritten.

Related: [config.conf](config-conf.md), [Installation](../getting-started/installation.md)
