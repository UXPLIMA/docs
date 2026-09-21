---
title: Commands
order: 20
description: What a player types, what an operator types, and how to rename any of it.
icon: terminal
---

Two roots: `/glow` for a player and `/uxmglow` for an operator. Both names, their aliases and
every word under them are values in `commands.conf`, so you may rename a command, give it
aliases, or turn it off entirely. A rename takes effect on the next start, because a server
builds its command tree once.

## What a player types

| Command | What it does | Permission |
|---|---|---|
| `/glow` | Opens the picker | `uxmglow.use` |
| `/glow set <colour>` | Glows in one colour | `uxmglow.use` and the colour's own node |
| `/glow cycle <colours> [interval-ms]` | Steps through several colours | `uxmglow.cycle` and every colour named |
| `/glow off` | Stops glowing | `uxmglow.use` |
| `/glow colours` | Lists the colours this player may use | `uxmglow.use` |
| `/glow lang` | Says which language they read this plugin in, and what else there is | `uxmglow.language` |
| `/glow lang set <language>` | Reads this plugin in one language, whatever the client says | `uxmglow.language` |
| `/glow lang reset` | Follows the client again | `uxmglow.language` |

The colours of a cycle are one comma-separated argument, `red,blue,gold`, so the interval that
may follow stays unambiguous. The interval is in milliseconds and it is optional: leave it out
and the plugin uses the default from `config.conf`.

Tab completion offers only the colours the sender actually holds. An interface that offers what
it then refuses is a bad one.

## What an operator types

| Command | What it does | Permission |
|---|---|---|
| `/uxmglow reload` | Re-reads `config.conf`, the picker and the language files | `uxmglow.admin.reload` |
| `/uxmglow version` | Reports the version | `uxmglow.admin.version` |
| `/uxmglow doctor` | Asks the plugin how it is: storage, content, windows, placeholders | `uxmglow.admin.doctor` |

## Renaming

`commands.conf` holds a block per command with a name and a list of aliases. Write the name you
want, restart, and the old one is gone. A command you do not want at all is turned off in the
same file, and it then leaves the tree rather than answering with a refusal.
