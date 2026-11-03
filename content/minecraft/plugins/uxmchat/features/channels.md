---
title: Channels
order: 401
description: Global, local and world channels, network and server scope, speaking and hearing, shortcuts, markers, locks and slowmode.
icon: hash
---

A channel decides who hears a line. Module: `channels`. Off, there is one public chat and `/channel` is gone.

## Kinds and scope

| Kind | Who hears a line |
|---|---|
| `global` | Everyone who hears the channel |
| `local` | Hearers within `radius` blocks of the speaker, in the same world |
| `world` | Hearers in the speaker's world, or in its world group |

| Scope | Reach |
|---|---|
| `network` | Every server of the network |
| `server` | This server only |

A world group is a list under `world-groups` in [config.conf](../config/config-conf.md#world-groups-and-time):
the shipped `survival` group joins `world`, `world_nether` and `world_the_end`, so a world channel line in the nether
reaches the overworld. A world in no group is its own group.

## The shipped channels

| Channel | Kind | Scope | Shortcut | Marker | Notes |
|---|---|---|---|---|---|
| `global` | global | network | `/g` | `!` | `@here` allowed; the default channel |
| `local` | local | server | `/l` | | radius 100 |
| `world` | world | server | `/w` | | |
| `trade` | global | network | `/trade` | `$` | 30 second slowmode |
| `help` | global | network | `/help-chat` | `?` | staff with `uxmchat.staff.listen` hear it without joining |
| `staff` | global | network | `/sc` | `#` | needs `uxmchat.channel.staff` to speak, hear and join; `@here` and `@everyone`; a hat sound |

Every shipped channel joins a player by itself (`auto-join`), so a player hears all of them they may read.

## Speaking and hearing

A player speaks in one channel at a time and hears many.

| Way | Effect |
|---|---|
| `/channel switch trade` | Speaks in trade from now on, and hears it |
| `/trade` | The same, through the shortcut |
| `/trade selling diamonds` | One line in trade; you keep speaking where you were |
| `$selling diamonds` | One line in trade through the marker |
| `$$` at the start | A plain `$`: a doubled marker is the character |
| `/channel join help` | Hears help too |
| `/channel leave trade` | Stops hearing trade |

The default channel, `chat.default-channel` (`global`), cannot be left. A channel's `send-permission` and
`read-permission` decide who may speak and hear; `auto-join-permission` decides who joins it by itself.

## Limits

| Limit | Set by | Bypass |
|---|---|---|
| Line length | the channel's `max-length`, or `chat.max-length` (256) | `uxmchat.bypass.length` |
| Slowmode | the channel's `slowmode-seconds`, or `/channel slowmode` | `uxmchat.bypass.slowmode` |
| Lock | `/channel lock` | `uxmchat.bypass.channel-lock` |

## Nobody heard you

With `chat.nobody-heard = true`, a player whose line reached nobody is told so, such as in an empty local channel.

## Staff

| Command | Does |
|---|---|
| `/channel lock <channel>`, `unlock` | Only staff with the bypass speak in it |
| `/channel slowmode <channel> <seconds>` | 0 to 3600 |
| `/channel force <player> <channel>` | Moves a player into a channel |
| `/channel kick <player> <channel>` | Takes them out of one |

## Where channels live

`channels.conf` is read once, on the first start, into the database; every server of a network then shares the
channels. Change them in game with `/chateditor channels`. A channel created in game gets its shortcut command at the
next start; until then `/channel switch` reaches it.

Related: [channels.conf](../config/channels-conf.md), [Formats](formats.md), [In-game editors](editors.md)
