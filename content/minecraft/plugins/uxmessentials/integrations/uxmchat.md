---
title: uxmChat
order: 1488
description: Which chat features uxmEssentials leaves to uxmChat when both are installed.
icon: message-square
---

With uxmChat on the same server, every chat feature both plugins have runs in uxmChat alone, so no line shows twice
and no command is registered twice. There is nothing to configure: uxmEssentials asks uxmChat which features it runs.

## What uxmChat takes over

| Feature | What leaves uxmEssentials |
|---|---|
| Chat format | The chat format of the Communication module |
| Join and quit lines | Join, quit, death and advancement lines |
| Announcements | The announcer |
| Private messages | `/msg`, `/reply`, `/msgtoggle`, `/rtoggle`, `/msgsettings` |
| Ignore | `/ignore`, `/unignore`, `/ignorelist` |
| Social spy | `/socialspy` |
| Mail | `/mail`, `/mailclear` |
| Helpop | `/helpop` |
| Broadcast | `/broadcast`, `/broadcasttoggle`, `/broadcastworld` |
| Clear chat | `/clearchat` |
| Me | `/me` |
| Chat toggle | `/togglechat` |
| Nicknames | `/nick`, `/realname` |
| Staff list | `/stafflist` |

## Notes

- **A feature uxmChat has turned off stays here.** uxmChat reports a feature only while its module is on and its
  command is enabled, so turning off uxmChat's mail gives `/mail` back to uxmEssentials.
- **Chat lines are decided on every event,** so the announcer speaks again here if uxmChat's announcements go off.
- **Commands are decided when the server builds its command tree,** after every plugin has enabled. A module
  switched on either side reaches the commands on the next command reload, not on a module reload alone.
- **A uxmChat that never started takes nothing.** Until the server ticks, an installed uxmChat counts as running
  every chat line; after that, one that failed to start runs nothing.
- **Kept here either way:** the joiner's MOTD, the page sent to a player who died, and `/mute`, which stays with
  [Moderation](../modules/moderation.md).
- **Vanished players are marked for uxmChat.** `/vanish` sets the `vanished` metadata other plugins read, so uxmChat
  leaves a vanished player out of its online count and friend notices.

Related: [Overview](overview.md), [Messaging](../modules/messaging.md), [Communication](../modules/communication.md)
