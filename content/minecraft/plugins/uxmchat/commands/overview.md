---
title: Overview
order: 301
description: Every command at a glance, the shortcuts and custom commands, and who sees what.
icon: list
---

Names and aliases are the defaults from [commands.conf](../config/commands-conf.md), which renames, aliases or turns
off each command and each subcommand. A command of a module that is off is not registered.

| Command | Aliases | Subcommands | Page |
|---|---|---|---|
| `/announcements` | | `preview`, `say` | [Staff](staff.md#announcements) |
| `/bossbar` | | | [Staff](staff.md#bossbar) |
| `/broadcast` | `/bc` | | [Staff](staff.md#broadcast) |
| `/channel` | `/ch` | `switch`, `join`, `leave`, `list`, `info`, `lock`, `unlock`, `slowmode`, `force`, `kick` | [Player](player.md#channel), [Staff](staff.md#channel) |
| `/chat` | | `settings`, `lang`, `lang set`, `lang reset` | [Player](player.md#chat) |
| `/chatcolor` | `/chatcolour` | `set`, `style`, `clear`, `list` | [Player](player.md#chatcolor) |
| `/chatdelete` | | | [Staff](staff.md#chatdelete) |
| `/chateditor` | | `channels`, `tags`, `announcements`, `announcement` | [Staff](staff.md#chateditor) |
| `/chatgame` | `/cg` | `top`, `answer`, `start` | [Player](player.md#chatgame), [Staff](staff.md#chatgame-start) |
| `/chatlock` | `/muteall`, `/lockchat` | | [Staff](staff.md#chatlock) |
| `/chatlog` | | `player`, `search`, `audit` | [Staff](staff.md#chatlog) |
| `/chatsnapshot` | | | [Player](player.md#chatsnapshot) |
| `/clearchat` | | `self` | [Player](player.md#clearchat-self), [Staff](staff.md#clearchat) |
| `/conversation` | `/convo` | | [Player](player.md#msg) |
| `/emoji` | `/emojis` | | [Player](player.md#emoji) |
| `/friend` | `/friends`, `/f` | `add`, `accept`, `deny`, `remove`, `favourite`, `list`, `requests` | [Player](player.md#friend) |
| `/helpop` | | | [Player](player.md#helpop) |
| `/ignore` | | `remove`, `list` | [Player](player.md#ignore) |
| `/macro` | | | [Staff](staff.md#macro) |
| `/mail` | | `read`, `send`, `clear` | [Player](player.md#mail) |
| `/me` | | | [Player](player.md#me) |
| `/msg` | `/tell`, `/w`, `/whisper`, `/m`, `/pm` | | [Player](player.md#msg) |
| `/mute` | | | [Staff](staff.md#mute) |
| `/nick` | `/nickname` | `off`, `player`, `realname` | [Player](player.md#nick), [Staff](staff.md#nick-player) |
| `/poll` | | `start`, `end` | [Staff](staff.md#poll) |
| `/quickreply` | `/qr` | `add`, `remove`, `list` | [Player](player.md#msg) |
| `/quote` | | | [Player](player.md#quote) |
| `/recent` | | | [Player](player.md#msg) |
| `/reply` | `/r` | | [Player](player.md#msg) |
| `/rules` | | `accept` | [Player](player.md#rules) |
| `/stafflist` | `/staff` | | [Player](player.md#stafflist) |
| `/tag` | `/tags` | `list`, `wear`, `clear`, `buy`, `favourite`, `search`, `make`, `pending`, `approve`, `deny`, `give`, `take` | [Player](player.md#tag), [Staff](staff.md#tag) |
| `/unignore` | | | [Player](player.md#ignore) |
| `/unmute` | | | [Staff](staff.md#mute) |
| `/uxmchat` | `/chatadmin` | `reload`, `version`, `doctor`, `lang set-player`, `lang reset-player`, `help` | [Admin](admin.md) |
| `/vote` | | | [Player](player.md#vote) |

`/mute` and `/unmute` are not registered when a punishment plugin keeps mutes; see
[Punishment plugins](../integrations/punishment-plugins.md).

## Shortcuts and custom commands

| Kind | Comes from | Example |
|---|---|---|
| Channel shortcuts | `shortcut` of each channel | `/g`, `/l`, `/w`, `/trade`, `/help-chat`, `/sc` |
| Custom commands | `modules/custom-commands/custom-commands.conf` | `/discord`, `/store` (`/shop`, `/buy`), `/report` |

`/g` alone moves you into global; `/g hello` says one line there and leaves you where you were. A channel created in
game gets its shortcut at the next start; until then `/channel switch` reaches it.

<Callout type="warning" title="/w is taken twice">

`/w` is an alias of `/msg` and the shortcut of the shipped `world` channel. Only one of them can own the word, so
pick one: remove `"w"` from the aliases of `msg` in `commands.conf`, or give the world channel another shortcut in
`/chateditor channels`.

</Callout>

## Who sees a command

A command or subcommand whose node is given to everyone by default (`default: true`) is listed for every player, and
a player whose rank had it taken away is told they may not use it. Every other node hides its command or subcommand
from a player who lacks it: they do not see it in the command list, in tab completion or in the help.

## Arguments

| Written | Means |
|---|---|
| `<player>` | A name, completed from the players online; network commands complete from every server |
| `[page]` | Optional, 1 when left out |
| `<text...>` | The rest of the line |

## Bedrock players

A window opens as a Bedrock form for a Bedrock player. Where a Java player clicks a line in chat (a quote button, a
recent sender, a poll answer), a Bedrock player runs the command it stands for, such as `/recent` or `/vote 2`.

Related: [commands.conf](../config/commands-conf.md), [Permissions](../permissions/reference.md)
