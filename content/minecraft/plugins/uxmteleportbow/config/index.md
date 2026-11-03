---
title: Configuration
order: 40
description: Every file the plugin writes, what belongs in it, reloading, and what an update does to your files.
icon: settings
---

The plugin writes its files into `plugins/uxmTeleportBow/` on first start; the folder is shown in
[Installation](../getting-started/installation.md#the-files). Every file is HOCON. Run `/uxmteleportbow reload`
(`uxmteleportbow.admin.reload`) after an edit; the few keys that need a restart say so on their page.

| Page | What it holds |
|---|---|
| [config.conf](config-conf.md) | Language, the bow and the arrow, the teleport rules, the trail, the effects, the filters |
| [commands.conf](commands-conf.md) | The name, aliases and switch of the command and its subcommands |
| [Messages](messages.md) | `messages/messages_<language>.conf`: every line the plugin writes, and the bow's name and lore |
| [Languages](languages.md) | The seven languages, adding one, and how a player's language is chosen |
| [theme.conf](theme.md) | The colours, glyphs and small capitals every UXM plugin shares |

## HOCON in two minutes

| Rule | Example |
|---|---|
| A block opens and closes with braces | `teleport { cooldown-ms = 3000 }` |
| `=` or `:` between key and value; a block needs neither | `cooldown-ms = 3000`, `trail { ... }` |
| Text in quotes, numbers and `true`/`false` without | `particle = "END_ROD"`, `count = 2` |
| Lists in square brackets, commas or new lines between | `list = ["spawn", "shop"]` |
| A comment starts with `#` | `# the default is 3000` |
| Two blocks of the same name merge | a `teleport { }` at the end adds to the one at the top |

A file that does not parse stops the reload, keeps the configuration that worked, and the console names the file
and the line. It is almost always a missing `}` or quote.

## Reloading

`/uxmteleportbow reload` reads `config.conf`, the messages and `theme.conf` again, and swaps the configuration in one
go, so a reload is never half applied. A file that does not parse, or a value out of its range (a negative cooldown,
`headroom-blocks = 0`, a slot above 40), stops the reload with `admin.reload-failed`, the console names the key, and
the server keeps the configuration that worked. The reply says how many milliseconds the reload took.

| Not read by a reload | Why |
|---|---|
| `commands.conf` | The server builds its command tree once, at start |
| `trail.tick-ms`, `trail.give-up-after-ms` | The drawing task is made once, at start |

A bow already in a player's hands keeps its material, glow and model until it is handed out again; its name and lore
follow the messages at once.

## What an update does

You never replace your files after an update. On the start after a new jar:

1. Every file the jar ships that is missing is written.
2. A key the release added is written into your `config.conf` with the comment that explains it, at its default.
3. A value you set is never changed.
4. Your file as it was goes to `backups/<file>-<date>.conf`; the last five copies of each file are kept.

The file is written again from what it holds, so your own formatting, such as alignment inside a block, may move. A
key you deleted comes back with its default, which is why every switch says in its comment what turning it off
means. An effects list you emptied stays empty.

Related: [config.conf](config-conf.md), [Installation](../getting-started/installation.md)
