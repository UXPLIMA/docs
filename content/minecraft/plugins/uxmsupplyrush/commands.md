---
title: Commands
order: 20
description: The player command and every admin command, with what each one needs.
icon: terminal
---

The command is `/supplyrush`, or `/sr` for short. Where a command takes an event, give its key
(shown on the manage menu) or its ID. Commands marked "running event" act on the event that is
running now when you leave the event out.

## Players

| Command | What it does |
|---|---|
| `/sr` | Opens the tournament menu |
| `/sr help` | Lists the commands the player can use. Without `uxmsupplyrush.command.help`, it opens the menu instead |

## Admins

| Command | What it does |
|---|---|
| `/sr events` | Lists every event, and creates new ones |
| `/sr manage [event]` | Opens an event's manage menu (running event). Also `/sr admin` |
| `/sr start <event>` | Starts an event, or resumes it if it is paused |
| `/sr pause` | Pauses the running event, keeping its scores |
| `/sr stop` | Ends the running event early, after asking whether to pay. Also `/sr complete`, `/sr finish` |
| `/sr duration <time> [event]` | Sets an event's length, such as `7d` or `12h30m` (running event) |
| `/sr delete <event>` | Deletes an event that is not running or paused, after asking |
| `/sr logs [event]` | Opens an event's logs, run by run (running event) |
| `/sr export [event] [run]` | Writes a summary file of an event, or of one of its finished runs, to `history/` (running event) |
| `/sr reload` | Reloads the configuration, event files and languages |
| `/sr management` | Shows which server manages a network |
| `/sr management claim` | Makes this server the management server, after asking |

`/sr stop` and `/sr delete` reply with clickable buttons in chat. They work once and expire after 60
seconds. See [Events and runs](features/events-and-runs.md#ending).

On a network, the commands that change events only work on the management server. See
[Multiple servers](features/multi-server.md#the-management-server).

Related: [Permissions](permissions.md)
