---
title: uxmEssentials
order: 904
description: Which chat features uxmEssentials hands to uxmChat, and which way mutes go.
icon: handshake
---

uxmEssentials steps back from every chat feature uxmChat runs:

| Feature | With uxmChat |
|---|---|
| Chat format | uxmChat's [formats](../features/formats.md) |
| Private messages, social spy | uxmChat's [private messages](../features/private-messages.md) and [spy](../features/staff-tools.md#spy) |
| Mail | uxmChat's [mail](../features/friends-ignore-mail.md#mail) |
| Helpop | uxmChat's `/helpop` |
| The announcer | uxmChat's [announcements](../features/announcements.md) |
| Join and quit lines | uxmChat's [server lines](../features/events.md) |
| Broadcast, clear chat, `/me` | uxmChat's |
| The chat toggle | uxmChat's [settings](../menus/settings.md) |

A feature turned off in uxmChat's `modules` stays with uxmEssentials.

Mutes go the other way: when uxmEssentials runs its moderation module, it keeps `/mute` and `/unmute`, and uxmChat
reads its mutes. See [Punishment plugins](punishment-plugins.md).

Related: [Punishment plugins](punishment-plugins.md)
