---
title: Configuration
order: 150
description: Every file the plugin writes, what belongs in it, reloading, and what an update does to your files.
icon: settings
---

The plugin writes its files into `plugins/uxmParkour/` on first start; the folder is shown in
[Installation](../getting-started/installation.md#the-files). Every file is HOCON. Run `/uxmparkour reload`
(`uxmparkour.admin.reload`) after an edit; the few keys that need a restart say so on their page.

| Page | What it holds |
|---|---|
| [config.conf](config-conf.md) | Language, storage, run rules, boards, difficulties, themes, moments, milestones, economy, session items, feedback, menu sounds |
| [arenas.conf](arenas-conf.md) | The arenas |
| [commands.conf](commands-conf.md) | The name, aliases and switch of every command |
| [Menu files](menu-files.md) | `menus/*.conf`: the three windows, and every key a window file takes |
| [Messages](messages.md) | `messages/messages_<language>.conf`: every line a player reads |
| [Languages](languages.md) | The seven shipped languages, adding one, and how a player's language is chosen |
| [theme.conf](theme.md) | The colours, glyphs and small capitals every UXM plugin shares |

## HOCON in two minutes

| Rule | Example |
|---|---|
| A block opens and closes with braces | `run { max-fall = 6 }` |
| `=` or `:` between key and value; a block needs neither | `max-fall = 6`, `run { ... }` |
| Text in quotes, numbers and `true`/`false` without | `type = "sqlite"`, `pool-size = 4` |
| Lists in square brackets, commas between | `periods = [ alltime, weekly ]` |
| A comment starts with `#` | `# the default is 6` |
| Two blocks of the same name merge | a `run { }` at the end adds to the one at the top |

A file that does not parse stops the reload, keeps the configuration that worked, and the console names the file
and the line. It is almost always a missing `}` or quote.

## Reloading

`/uxmparkour reload` reads `config.conf`, the messages, `theme.conf`, the window files and the menu sounds
again, and swaps the whole configuration in one go, so nothing runs on half a file. A value that is wrong, such as a difficulty with no
possible jump or two milestones on one count, stops the reload with the reason in the console, and the server keeps
running on the last configuration that worked. The reply says how many milliseconds the reload took.

| Needs a restart | Why |
|---|---|
| `commands.conf` | The server builds its command tree once, at start |
| `leaderboard.refresh-seconds` | The board timer is started once |
| `storage` | The database is opened once |

`arenas.conf` is not part of a reload. An edit by command or editor is written at once; a change made by hand in the
file is read at a restart, or the next time any arena is saved, since a save reads the file before it writes.

## What an update does

You never replace your files after an update. On the start after a new jar:

1. Every file the jar ships that is missing is written.
2. A key the release added is written into your file with the comment that explains it, at its default.
3. A value you set is never changed.
4. Your file as it was goes to `backups/<file>-<date>.conf`; the last five copies of each file are kept.
5. The database moves forward on its own.

The file is written again from what it holds, so your own formatting, such as alignment inside a block, may
move. A key you deleted comes back with its default, which is why every switch says
in its comment what turning it off means. The lists you own are the exception and stay as you left them: the
difficulties, the themes and the milestones.

Related: [config.conf](config-conf.md), [Installation](../getting-started/installation.md)
