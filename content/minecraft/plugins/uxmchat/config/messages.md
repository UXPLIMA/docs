---
title: Messages
order: 615
description: How a line is written, the prefixes, every block of keys, and language keys used in content.
icon: message-square
---

Every line comes from `messages/messages_<language>.conf`. A line is MiniMessage and names roles, not colours; the
colours are in [theme.conf](theme.md). A key missing from a file falls back to the default language, then to the line
the code carries.

| Prefix | Draws |
|---|---|
| `<tag:'CHAT'>` | A category word: `CHAT`, `CHANNEL`, `MSG`, `FRIEND`, `MAIL`, `TAG`, `COLOUR`, `MUTE`, `POLL`, `GAME`... |
| `<etag:'ERROR'>` | An error |
| `<h:'...'>` | A header |

## Blocks

| Block | Said when |
|---|---|
| `chat` | A line is refused: not logged in, muted, locked, rules first, slowmode, too long, a filter's reason |
| `staff` | Staff notices: filter catches, waiting tags, mutes, AI reviews, helpop |
| `mention` | How a mention tells the player: action bar, title, boss bar |
| `channel`, `channel-name` | `/channel` replies, and the names of the shipped channels |
| `pm`, `conversation`, `quick`, `me` | Private messages, conversations, quick replies, `/me` |
| `ignore`, `friend`, `mail`, `nick` | The social commands |
| `tag`, `colour` | Tags and chat colours; `tag.worn`, `tag.cleared`, `tag.lapsed`, `colour.set`... are the effects' lines |
| `snapshot` | Snapshot tokens, window titles, the Bedrock form |
| `mute`, `lock`, `clear`, `delete`, `broadcast`, `bossbar` | Staff actions |
| `poll`, `rules`, `macro`, `helpop`, `staff-list` | Those features |
| `log`, `audit`, `spy` | The chat log, staff actions, copies |
| `auto-reply` | The automatic answers |
| `announcements`, `announce` | `/announcements` replies, and the shipped announcement lines |
| `events`, `network` | Server lines, staff network moves |
| `format` | The newcomer badge and the format hovers |
| `game` | Chat game rounds and the winners' table |
| `menu` | Every window's words |
| `editor` | The in-game editors |
| `currency` | How an amount is written |
| `time` | `forever` |
| `admin`, `language`, `command` | `/uxmchat`, `/chat lang`, the command layer |

## Your own lines in content

A tag, category, rarity, channel name, colour name, announcement line, event line, bubble format, automatic reply or
currency format may name a line as `@key`. Add your own under a block of your own in every language file:

```hocon
custom {
  tag {
    vip = "<#FFE66D>[VIP]"
  }
}
```

and write `display = "@custom.tag.vip"`. Each reader sees it in their own language.

Related: [Languages](languages.md), [theme.conf](theme.md)
