---
title: Configuration
order: 140
description: Every file the plugin writes, what belongs in it, reloading, and what an update does to your files.
icon: settings
---

The plugin writes its files into `plugins/uxmHubPvP/` on first start; the folder is shown in
[Installation](../getting-started/installation.md#the-files). Every file is HOCON. Run `/uxmhubpvp reload`
(`uxmhubpvp.admin.reload`) after an edit; the few keys that need a restart say so on their page.

| Page | What it holds |
|---|---|
| [config.conf](config-conf.md) | Language, storage, arena rules, warm-ups, combat tag, toggle item, respawn, visuals, rewards, economy, boards, moments |
| [kit.conf](kit-conf.md) | The kit a fighter is handed |
| [commands.conf](commands-conf.md) | The name, aliases and switch of every command |
| [Messages](messages.md) | `messages/messages_<language>.conf`: every line a player reads |
| [Languages](languages.md) | The seven languages, adding one, and how a player's language is chosen |
| [theme.conf](theme.md) | The colours, glyphs and small capitals every UXM plugin shares |

## HOCON in two minutes

| Rule | Example |
|---|---|
| A block opens and closes with braces | `warm-up { enter-ms = 3000 }` |
| `=` or `:` between key and value; a block needs neither | `enter-ms = 3000`, `warm-up { ... }` |
| Text in quotes, numbers and `true`/`false` without | `type = "sqlite"`, `pool-size = 4` |
| Lists in square brackets, commas between | `periods = [ alltime, weekly ]` |
| A comment starts with `#` | `# the default is 3000` |
| Two blocks of the same name merge | a `warm-up { }` at the end adds to the one at the top |

A file that does not parse stops the reload, keeps the configuration that worked, and the console names the file
and the line. It is almost always a missing `}` or quote.

## Reloading

`/uxmhubpvp reload` reads `config.conf`, `kit.conf`, the messages and `theme.conf` again. The configuration is swapped
in one go, so nothing runs on half a file. A value that is wrong, such as a toggle slot of 9 or a money line that
cannot be read, stops the reload with `admin.reload-failed` and the reason in the console, and the server keeps
running on the last configuration that worked. The reply says how many milliseconds the reload took.

| Needs a restart | Why |
|---|---|
| `commands.conf` | The server builds its command tree once, at start |
| `leaderboard.refresh-seconds` | The board timer is started once |
| `storage` | The database is opened once |

A fighter keeps the kit they were given; the next player to enter gets the new one.

## What an update does

You never replace your files after an update. On the start after a new jar:

1. Every file the jar ships that is missing is written.
2. A key the release added is written into your `config.conf` with the comment that explains it, at its default.
3. A value you set is never changed.
4. Your file as it was goes to `backups/<file>-<date>.conf`; the last five copies of each file are kept.
5. The database moves forward on its own.

The file is written again from what it holds, so your own formatting, such as alignment inside a block, may move. A
key you deleted comes back with its default, which is why every switch says in its comment what turning it off
means. `kit.conf` is yours and an update never merges it: a piece you removed stays removed.

Related: [config.conf](config-conf.md), [Installation](../getting-started/installation.md)
