---
title: Reference
order: 801
description: Every %uxmchat_...% placeholder, its relational form, and what an empty value means.
icon: list
---

PlaceholderAPI is required; the identifier is `uxmchat`. Formats read other plugins' placeholders when PlaceholderAPI
is installed, and uxmChat answers its own for tab lists, scoreboards and other plugins. Coloured values come with
section codes and hex colours, and the `_mini` forms with MiniMessage for a plugin that parses it. A player who is not
online on that server reads as empty, except `%uxmchat_server%`.

| Placeholder | Value |
|---|---|
| `%uxmchat_channel%` | The id of the channel the player talks in |
| `%uxmchat_channel_name%` | That channel's coloured name |
| `%uxmchat_channel_name_mini%` | That name as MiniMessage |
| `%uxmchat_tag%` | The coloured tag the player shows, or empty |
| `%uxmchat_tag_mini%` | That tag as MiniMessage, or empty |
| `%uxmchat_tag_plain%` | That tag without formatting, or empty |
| `%uxmchat_has_tag%` | Whether the player shows a tag: `true` or `false` |
| `%uxmchat_tag_id%` | The id of the tag the player chose, or empty |
| `%uxmchat_nick%` | The coloured nick, or the name |
| `%uxmchat_colour%` | The id of the chat colour, or empty; `%uxmchat_color%` too |
| `%uxmchat_muted%` | `true` or `false` |
| `%uxmchat_messages%` | Whether private messages are on: `true` or `false` |
| `%uxmchat_server%` | The server the player is on in the network |
| `%uxmchat_online%` | How many players the network shows online |

## Languages

A tag or a channel name written as a language key reads in the language of the player it is about. Every placeholder
also answers as `%rel_uxmchat_<name>%`, which reads it in the language of the player the chat line, tab row or nametag
is drawn for: `%rel_uxmchat_tag_mini%` in another plugin's nametag shows each viewer the tag in their own language.

## Examples

```
Tab list prefix:  %uxmchat_tag%
Nametag (relational):  %rel_uxmchat_tag_mini%
Scoreboard:  Channel: %uxmchat_channel_name%
Scoreboard:  Online: %uxmchat_online%
```

Related: [Tags](../features/tags.md#surfaces), [A network](../features/network.md)
