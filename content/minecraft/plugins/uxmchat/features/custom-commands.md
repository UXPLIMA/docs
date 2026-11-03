---
title: Custom commands
order: 314
description: Commands of your own with permissions, argument counts, conditions, actions, cooldowns and argument tokens.
icon: square-terminal
---

Module: `custom-commands`. Each block of `modules/custom-commands/custom-commands.conf` is a command. Read at start; a
change needs a restart.

| Key | Meaning |
|---|---|
| `enabled` | `false` leaves it out |
| `aliases` | More names for it |
| `description` | Shown in the command list |
| `permission` | Needed to run it; empty lets everyone |
| `min-args`, `max-args` | How many words may follow it; `max-args = -1` has no limit |
| `usage` | The line a player reads with the wrong number of words; `@` for a messages line |
| `conditions` | uxmLib condition lines; all must hold |
| `actions` | uxmLib action lines run when they hold |
| `denied` | Action lines run when one does not |
| `cooldown-seconds` | How long one player waits between two runs |

| Token | Value |
|---|---|
| `{player}` | The player's name |
| `{args}` | Every word |
| `{arg1}`, `{arg2}`... | One word each |
| `{args2}`, `{args3}`... | Every word from that one on |

A word holds only letters, digits, `_`, `.` and `-`; anything else is dropped, so a word can never become a second
command.

## Shipped

| Command | Does |
|---|---|
| `/discord` | A clickable Discord invite |
| `/store` (`/shop`, `/buy`) | A clickable store link |
| `/report <player> <reason>` | Runs `helpop Report on {arg1}: {args2}` as the player and thanks them; node `uxmchat.command.report`, 60 second cooldown |

Related: [custom-commands.conf](../config/custom-commands-conf.md)
