---
title: Mutes
order: 406
description: uxmChat's own mutes, the punishment plugins it hands mutes to, and the chat lock.
icon: volume-x
---

## Who keeps mutes

| On the server | `/mute` and `/unmute` | Mute records |
|---|---|---|
| No punishment plugin | uxmChat's own (module `mutes`) | uxmChat's database |
| uxmEssentials with its moderation module, LiteBans, LibertyBans, AdvancedBan or Essentials | That plugin's | That plugin's |

With a punishment plugin installed and module `punishment-plugins` on, uxmChat registers neither command. It reads that
plugin's mutes to keep muted players quiet, except LibertyBans', whose own chat block does that, and when the ladder
of the filters calls for a mute it runs the plugin's own command. See
[Punishment plugins](../integrations/punishment-plugins.md).

## uxmChat's own mutes

`/mute <player> [time] [reason]`, node `uxmchat.mute`. A time is `30s`, `10m`, `2h`, `1d`, `1w` or a mix such as
`1h30m`; a first word that is not a time starts the reason, and the mute has no end. `/unmute <player>` lifts it.

A mute reaches the player on any server of the network. The muted player reads how long is left and why; staff with
`uxmchat.staff.mute` are told who muted whom. Every mute and unmute is in `/chatlog audit`.

## Locking the chat

`/chatlock` closes the public chat on the network for everyone but those with `uxmchat.bypass.lock`, and opens it
again. `/channel lock` closes one channel.

Related: [Staff tools](staff-tools.md), [Filters](filters.md#the-ladder)
