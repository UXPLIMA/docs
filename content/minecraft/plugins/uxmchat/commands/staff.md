---
title: Staff commands
order: 203
description: Moderation, channel control, announcements, editors, polls and every other staff command, with its node.
icon: shield
---

Every node on this page is given to operators by default and hidden from everybody else.

## /mute

| Command | Node | What it does |
|---|---|---|
| `/mute <player> [time] [reason...]` | `uxmchat.mute` | Stops a player from talking. A time is `30s`, `10m`, `2h`, `1d`, `1w` or a mix such as `1h30m`; without one the mute has no end |
| `/unmute <player>` | `uxmchat.mute` | Lets them talk again |

A first word that is not a time starts the reason. Both commands exist only when uxmChat keeps mutes itself; see
[Mutes](../features/mutes.md).

## /channel

| Command | Node | What it does |
|---|---|---|
| `/channel lock <channel>` | `uxmchat.channel.lock` | Nobody speaks in it but those with `uxmchat.bypass.channel-lock` |
| `/channel unlock <channel>` | `uxmchat.channel.lock` | Opens it again |
| `/channel slowmode <channel> <seconds>` | `uxmchat.channel.slowmode` | The least time between two lines of one player, 0 to 3600; 0 turns it off |
| `/channel force <player> <channel>` | `uxmchat.channel.force` | Moves a player into a channel |
| `/channel kick <player> <channel>` | `uxmchat.channel.kick` | Takes a player out of a channel |

## /chatlock

`/chatlock` (aliases `/muteall`, `/lockchat`, node `uxmchat.chatlock`) closes the public chat on the network for
everyone without `uxmchat.bypass.lock`, or opens it again.

## /clearchat

`/clearchat` (node `uxmchat.clearchat`) clears everyone's chat on the network. Players with `uxmchat.bypass.clear`
keep theirs.

## /chatdelete

`/chatdelete <line>` (node `uxmchat.staff.delete`) removes a line for everyone. It is what the remove button after
each line runs for staff; the line is the id the button carries.

## /chatlog

| Command | Node | What it does |
|---|---|---|
| `/chatlog player <player> [page]` | `uxmchat.chatlog` | The lines of one player, newest first |
| `/chatlog search <words...>` | `uxmchat.chatlog` | Every line that holds the words |
| `/chatlog audit [page]` | `uxmchat.audit` | The staff actions: mutes, deletes, clears, locks, tag reviews |

Lines are kept `chat-log.retention-days` (30).

## /broadcast

`/broadcast <text...>` (alias `/bc`, node `uxmchat.broadcast`) is a MiniMessage line for every player on the network.

## /bossbar

`/bossbar <player|all> <seconds> <colour> <text...>` (node `uxmchat.bossbar`) shows a bar at the top of the screen for
1 to 3600 seconds. Colours are `pink`, `blue`, `red`, `green`, `yellow`, `purple` and `white`.

## /poll

| Command | Node | What it does |
|---|---|---|
| `/poll start <seconds> <question \| answer \| answer...>` | `uxmchat.poll` | Asks the network, for 10 to 3600 seconds; the question and the answers are split by `\|` |
| `/poll end` | `uxmchat.poll` | Ends the running poll and says the result |

Example: `/poll start 60 Next event? | Build contest | PvP tournament | Parkour`.

## /macro

`/macro <name>` (node `uxmchat.macro`) says one of the lines under `macros` in `config.conf` in your channel, as if
you typed it.

## /tag

| Command | Node | What it does |
|---|---|---|
| `/tag pending` | `uxmchat.staff.tags` | Player-made tags waiting for approval |
| `/tag approve <tag>` | `uxmchat.staff.tags` | Approves one, so its maker may wear it |
| `/tag deny <tag>` | `uxmchat.staff.tags` | Turns one down |
| `/tag give <player> <tag>` | `uxmchat.tag.give` | Gives a tag, to a player on any server or offline |
| `/tag take <player> <tag>` | `uxmchat.tag.give` | Takes a given tag back |

A tag held through a permission or a group is not taken by `take`: take the permission instead.

## /nick player

| Command | Node | What it does |
|---|---|---|
| `/nick player <player> <nick>` | `uxmchat.nick.others` | Sets another player's nickname |
| `/nick realname <nick>` | `uxmchat.nick.realname` | Who goes by that nickname |

## /chateditor

| Command | Node | What it does |
|---|---|---|
| `/chateditor channels` | `uxmchat.editor.channels` | The channel editor |
| `/chateditor tags` | `uxmchat.editor.tags` | The tag editor; action lines need `uxmchat.editor.actions` too |
| `/chateditor announcements` | `uxmchat.editor.announcements` | The announcement bundles |
| `/chateditor announcement` | `uxmchat.editor.announcements` | Each announcement on its own, with every field |

See [In-game editors](../features/editors.md).

## /announcements

| Command | Node | What it does |
|---|---|---|
| `/announcements` | `uxmchat.editor.announcements` | Every announcement as `bundle/id`, where it shows and whether it is on |
| `/announcements preview <bundle/id>` | `uxmchat.editor.announcements` | Shows you one as a player sees it |
| `/announcements say <bundle/id>` | `uxmchat.editor.announcements` | Says it now |

## /chatgame start

`/chatgame start <kind>` (node `uxmchat.chatgame.start`) starts a round now: `unscramble`, `math`, `trivia`,
`first-to-type`, `reaction`, `fill-blank` or `reversed`.

Related: [Admin commands](admin.md), [Permissions](../permissions/reference.md#staff)
