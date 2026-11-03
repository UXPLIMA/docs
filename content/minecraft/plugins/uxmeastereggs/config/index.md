---
title: Configuration
order: 150
description: Every file the plugin writes, what belongs in it, reloading, and what an update does to your files.
icon: settings
---

The plugin writes its files into `plugins/uxmEasterEggs/` on first start; the folder is shown in
[Installation](../getting-started/installation.md#the-files). Every file is HOCON. Run `/uxmeastereggs reload`
(`uxmeastereggs.admin.reload`) after an edit; the few keys that need a restart say so on their page.

| Page | What it holds |
|---|---|
| [config.conf](config-conf.md) | Language, storage, finds, radar, particles, economy, milestones, completion, boards, menus, moments |
| [Type files](type-files.md) | `types/<type>.conf`: one egg type each |
| [commands.conf](commands-conf.md) | The name, aliases and switch of every command |
| [Menu files](menu-files.md) | `menus/*.conf`: the four windows, and every key a window file takes |
| [Messages](messages.md) | `messages/messages_<language>.conf`: every line a player reads |
| [Languages](languages.md) | The seven languages, adding one, and how a player's language is chosen |
| [theme.conf](theme.md) | The colours, glyphs and small capitals every UXM plugin shares |

## HOCON in two minutes

| Rule | Example |
|---|---|
| A block opens and closes with braces | `radar { radius = 48 }` |
| `=` or `:` between key and value; a block needs neither | `radius = 48`, `radar { ... }` |
| Text in quotes, numbers and `true`/`false` without | `type = "sqlite"`, `pool-size = 4` |
| Lists in square brackets, commas between | `periods = [ alltime, weekly ]` |
| A comment starts with `#` | `# the default is 48` |
| Two blocks of the same name merge | a `radar { }` at the end adds to the one at the top |

A file that does not parse stops the reload, keeps the configuration that worked, and the console names the file
and the line. It is almost always a missing `}` or quote.

## Reloading

`/uxmeastereggs reload` reads `config.conf`, every type file, the messages, `theme.conf`, the window files and the menu
sounds again, and restarts the radar or the particle task only when its interval changed. The configuration is
swapped in one go, so nothing runs on half a file. A value that is wrong, such as two milestones at one number or a
money line that cannot be read, stops the reload with `admin.reload-failed` and the reason in the console, and the
server keeps running on the last configuration that worked. A type file that cannot be read is skipped and the
others load. The reply says how many milliseconds the reload took.

| Needs a restart | Why |
|---|---|
| `commands.conf` | The server builds its command tree once, at start |
| `leaderboard.refresh-seconds` | The board timer is started once |
| `storage` | The database is opened once |
| `radar.default-on` | The radar's starting state is read once, at start |

## What an update does

You never replace your files after an update. On the start after a new jar:

1. Every file the jar ships that is missing is written.
2. A key the release added is written into your `config.conf` with the comment that explains it, at its default.
3. A value you set is never changed.
4. Your file as it was goes to `backups/<file>-<date>.conf`; the last five copies of each file are kept.
5. The database moves forward on its own.

The file is written again from what it holds, so your own formatting, such as alignment inside a block, may move. A
key you deleted comes back with its default, which is why every switch says in its comment what turning it off
means. The milestones are the exception and stay as you left them, and the type files and menu files are yours.

Related: [config.conf](config-conf.md), [Installation](../getting-started/installation.md)
