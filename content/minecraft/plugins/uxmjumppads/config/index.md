---
title: Configuration
order: 140
description: Every file the plugin writes, what belongs in it, reloading, and what an update does to your files.
icon: settings
---

The plugin writes its files into `plugins/uxmJumpPads/` on first start; the folder is shown in
[Installation](../getting-started/installation.md#the-files). Every file is HOCON. Run `/uxmjumppads reload`
(`uxmjumppads.admin.reload`) after an edit; the few keys that need a restart say so on their page.

| Page | What it holds |
|---|---|
| [config.conf](config-conf.md) | Language, the pad types, the cooldown, the fall grace |
| [pads.conf](pads-conf.md) | The pads set on single blocks |
| [commands.conf](commands-conf.md) | The name, aliases and switch of every command |
| [Messages](messages.md) | `messages/messages_<language>.conf`: every line the plugin writes |
| [Languages](languages.md) | The seven languages, adding one, and how a player's language is chosen |
| [theme.conf](theme.md) | The colours, glyphs and small capitals every UXM plugin shares |

## HOCON in two minutes

| Rule | Example |
|---|---|
| A block opens and closes with braces | `fall-damage { grace-ms = 10000 }` |
| `=` or `:` between key and value; a block needs neither | `cooldown-ms = 750`, `fall-damage { ... }` |
| Text in quotes, numbers and `true`/`false` without | `type = "sqlite"`, `pool-size = 4` |
| Lists in square brackets, commas between | `periods = [ alltime, weekly ]` |
| A comment starts with `#` | `# the default is 750` |
| Two blocks of the same name merge | a `pads { }` at the end adds to the one at the top |

A file that does not parse stops the reload, keeps the configuration that worked, and the console names the file
and the line. It is almost always a missing `}` or quote.

## Reloading

`/uxmjumppads reload` reads `config.conf`, the messages and `theme.conf` again, and swaps the configuration in one go.
A pad type that cannot be read is left out and named in the console and in `doctor`; the others keep working. A file
that does not parse stops the reload with `admin.reload-failed`, and the server keeps the configuration that worked.
The reply says how many milliseconds the reload took.

| Not read by a reload | Why |
|---|---|
| `commands.conf` | The server builds its command tree once, at start |
| `pads.conf` | It is data the plugin writes; it is read at start |

## What an update does

You never replace your files after an update. On the start after a new jar:

1. Every file the jar ships that is missing is written.
2. A key the release added is written into your `config.conf` with the comment that explains it, at its default.
3. A value you set is never changed.
4. Your file as it was goes to `backups/<file>-<date>.conf`; the last five copies of each file are kept.

The file is written again from what it holds, so your own formatting, such as alignment inside a block, may move. A
key you deleted comes back with its default, which is why every switch says in its comment what turning it off
means. The `pads` map is the exception and stays as you left it.

Related: [config.conf](config-conf.md), [Installation](../getting-started/installation.md)
