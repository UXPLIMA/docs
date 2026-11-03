---
title: Configuration
order: 140
description: Every file the plugin writes, what belongs in it, reloading, and what an update does to your files.
icon: settings
---

The plugin writes its files into `plugins/uxmPlayerHider/` on first start; the folder is shown in
[Installation](../getting-started/installation.md#the-files). Every file is HOCON. Run `/uxmplayerhider reload`
(`uxmplayerhider.admin.reload`) after an edit; the few keys that need a restart say so on their page.

| Page | What it holds |
|---|---|
| [config.conf](config-conf.md) | Language, storage, worlds, the cooldown, the tool, the effects of each state |
| [commands.conf](commands-conf.md) | The name, aliases and switch of every command |
| [Messages](messages.md) | `messages/messages_<language>.conf`: every line the plugin writes, and the tool's words |
| [Languages](languages.md) | The seven languages, adding one, and how a player's language is chosen |
| [theme.conf](theme.md) | The colours, glyphs and small capitals every UXM plugin shares |

## HOCON in two minutes

| Rule | Example |
|---|---|
| A block opens and closes with braces | `cooldown { ms = 2000 }` |
| `=` or `:` between key and value; a block needs neither | `slot = 7`, `tool { ... }` |
| Text in quotes, numbers and `true`/`false` without | `material = "LIME_DYE"`, `glow = false` |
| Lists in square brackets, commas or new lines between | `worlds = ["lobby", "hub"]` |
| A comment starts with `#` | `# the default is 2000` |
| Two blocks of the same name merge | a `tool { }` at the end adds to the one at the top |

A file that does not parse stops the reload, keeps the configuration that worked, and the console names the file
and the line. It is almost always a missing `}` or quote.

## Reloading

`/uxmplayerhider reload` reads `config.conf`, the messages and `theme.conf` again, and swaps the configuration in one
go, so a reload is never half applied. A file that does not parse, or a value out of its range (a negative cooldown,
a tool slot outside 0 to 8, a state without an icon), stops the reload with `admin.reload-failed`, and the server
keeps the configuration that worked. The reply says how many milliseconds the reload took.

A reload changes what happens next: the next change, join or world change reads the new values. It does not apply a
changed `worlds` list, or redraw the tool's icon, for the players already online until one of those happens to them.
The tool's words follow the messages at once.

| Not read by a reload | Why |
|---|---|
| `commands.conf` | The server builds its command tree once, at start |
| `storage` | The database is opened once, at start |

## What an update does

You never replace your files after an update. On the start after a new jar:

1. Every file the jar ships that is missing is written.
2. A key the release added is written into your `config.conf` with the comment that explains it, at its default.
3. A value you set is never changed.
4. Your file as it was goes to `backups/<file>-<date>.conf`; the last five copies of each file are kept.

The file is written again from what it holds, so your own formatting, such as alignment inside a block, may move. A
key you deleted comes back with its default, which is why every switch says in its comment what turning it off
means. An effects list you emptied stays empty; only a list you delete comes back.

Related: [config.conf](config-conf.md), [Installation](../getting-started/installation.md)
