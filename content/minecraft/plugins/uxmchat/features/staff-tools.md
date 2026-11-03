---
title: Staff tools
order: 416
description: Spy, the chat log and staff actions, removing lines, clearing and locking, broadcast, boss bars, polls, rules, helpop, macros and the staff list.
icon: shield
---

## Spy

Module: `spies`. Staff with spy turned on in their [settings](../menus/settings.md) read copies:

| Node | Copies of |
|---|---|
| `uxmchat.spy.pm` | Private messages |
| `uxmchat.spy.command` | Commands; the words of `spy.secret-commands` (`login`, `l`, `register`, `reg`, `changepassword`, `changepass`, `unregister`) show only as the command name with `***` |
| `uxmchat.spy.surface` | Signs, books and item names |

## The chat log

Module: `chat-log`. Every line is kept `chat-log.retention-days` (30) with its server and channel.

| Command | Node |
|---|---|
| `/chatlog player <player> [page]` | `uxmchat.chatlog` |
| `/chatlog search <words>` | `uxmchat.chatlog` |
| `/chatlog audit [page]` | `uxmchat.audit`: mutes, deletes, clears, locks and the other staff actions |

## Removing a line

Module: `staff-delete`. Staff with `uxmchat.staff.delete` see `[x]` after each line; a click removes it for everyone.

## Clearing and locking

| Command | Node | Bypass |
|---|---|---|
| `/clearchat` | `uxmchat.clearchat` | `uxmchat.bypass.clear` keeps a chat |
| `/clearchat self` | `uxmchat.clearchat.self` | |
| `/chatlock` | `uxmchat.chatlock` | `uxmchat.bypass.lock` speaks while closed |

## Broadcast and boss bars

`/broadcast <text>` sends a MiniMessage line to every server. `/bossbar <player|all> <seconds> <colour> <text>` shows
a bar for 1 to 3600 seconds.

## Polls

Module: `polls`. `/poll start <seconds> <question | answer | answer>` asks the network for 10 to 3600 seconds; each
answer is a clickable `[1]`, `[2]`... that runs `/vote <n>`. At the end, or at `/poll end`, each answer's votes and
share are said. One poll runs at a time, with up to twenty answers.

## Rules

Module: `rules`. With `rules.version` above 0, a player must read `/rules` and click `[I accept the rules]` before
they may chat. Raise the version after changing `rules.lines` and every player is asked again. `uxmchat.bypass.rules`
skips it.

## Helpop

Module: `helpop`. `/helpop <question>` reaches staff with `uxmchat.staff.helpop` on every server, with the server it
came from. One question every `helpop.cooldown-seconds` (60).

## Macros

Module: `macros`. `/macro <name>` says a line from `macros` in `config.conf` in the staff member's channel, as if they
typed it: `discord` and `rules` ship.

## The staff list

Module: `staff-list`. `/stafflist` lists the staff (`uxmchat.staff`) online on every server. Vanished staff appear only
for those with `uxmchat.stafflist.vanished`. Staff are also told when a staff member comes onto a server or moves.

## /me

Module: `me`. `/me <action>` says `* Name action` in the player's channel, through the same gates and filters.

## Login plugins

Module: `auth`. With AuthMe, nLogin or LibreLogin, a player may not chat until they log in.

Related: [Staff commands](../commands/staff.md), [Permissions](../permissions/reference.md#staff)
