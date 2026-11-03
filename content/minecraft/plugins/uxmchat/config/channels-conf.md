---
title: channels.conf
order: 503
description: Every key of a channel and the six shipped channels.
icon: hash
---

Read once, on the first start, and again only if the channels table is empty. After that the channels live in the
database and are changed in game with `/chateditor channels`. Editing this file later changes nothing.

| Key | Meaning |
|---|---|
| `display-name` | How the channel is shown, MiniMessage, or `@key` for a line of the messages files |
| `kind` | `global`, `local` (within `radius` blocks, same world) or `world` (same world or world group) |
| `scope` | `network` (every server) or `server` (this server only) |
| `shortcut` | A command that speaks in the channel, such as `/g hello`; empty for none |
| `marker` | A line that starts with it goes to the channel, such as `!hello`; doubled it is a plain character |
| `radius` | Blocks, for a local channel |
| `send-permission` | Needed to speak; empty lets everyone |
| `read-permission` | Needed to hear; empty lets everyone |
| `max-length` | The longest line; 0 takes `chat.max-length` |
| `slowmode-seconds` | The least time between two lines of one player |
| `auto-join` | Whether a player hears it without joining |
| `auto-join-permission` | Needed for auto-join; empty lets everyone |
| `sound` | Played to every hearer, such as `minecraft:block.note_block.hat`; empty for none |
| `allow-here` | Whether `@here` works in it |
| `allow-everyone` | Whether `@everyone` works in it |
| `staff-listen` | Whether staff with `uxmchat.staff.listen` hear it without joining |
| `format` | A format from `formats.conf`; empty takes the group or default format |
| `group-formats` | A format per group, inside this channel only |

```hocon
staff {
  display-name = "@channel-name.staff"
  kind = "global"
  scope = "network"
  shortcut = "sc"
  marker = "#"
  send-permission = "uxmchat.channel.staff"
  read-permission = "uxmchat.channel.staff"
  auto-join = true
  auto-join-permission = "uxmchat.channel.staff"
  allow-here = true
  allow-everyone = true
  sound = "minecraft:block.note_block.hat"
  format = "staff"
}
```

The six shipped channels are listed in [Channels](../features/channels.md#the-shipped-channels).

Related: [Channels](../features/channels.md), [In-game editors](../features/editors.md)
