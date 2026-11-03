---
title: Formats
order: 302
description: How a line is drawn from parts, the placeholders, hovers and clicks, and which format a line takes.
icon: brush
---

A format is a list of parts. Each part has a `name`, a `template`, and an optional `hover` and `click`, so the name
can open a private message and the prefix can show a rank's perks.

```hocon
global {
  parts = [
    { name = "channel", template = "<dim>[G]</dim> ", hover = "@format.hover.global", click = "suggest:/channel switch global" }
    { name = "tag",     template = "<tag>" }
    { name = "prefix",  template = "<newcomer><prefix>" }
    { name = "name",    template = "<display>", hover = "@format.hover.message-server", click = "suggest:/msg <sender> " }
    { name = "message", template = "<dim>: </dim><message>" }
  ]
}
```

## Placeholders

| Placeholder | Value |
|---|---|
| `<message>` | The player's words, after filters, colour, emoji, tokens and mentions |
| `<sender>` | The account name |
| `<display>` | The nick, or the account name |
| `<prefix>`, `<suffix>` | The permission prefix and suffix (LuckPerms) |
| `<group>` | The deciding group |
| `<tag>` | The worn tag, in the `wrapper` of `tags.conf` |
| `<channel>` | The channel's display name |
| `<channel_id>` | The channel's id |
| `<world>` | The speaker's world |
| `<server>` | The speaker's server |
| `<newcomer>` | The newcomer badge, for a new player |
| `<meta:key>` | Any LuckPerms meta value |
| `%placeholder%` | Any PlaceholderAPI placeholder |

## Clicks

| Click | Does |
|---|---|
| `suggest:<text>` | Puts the text in the reader's chat box |
| `run:<command>` | Runs the command as the reader |
| `url:<link>` | Opens a link |
| `copy:<text>` | Copies the text |

`<sender>` and `<channel_id>` work inside a click.

## Language keys

A template or hover that starts with `@` is that line of the messages files, drawn in each reader's language. The
shipped hovers are the `format.hover` lines.

## Which format a line takes

Most specific first:

1. The channel's format for the speaker's group (`group-formats` of the channel).
2. The channel's `format`.
3. The group's format (`group-formats` of `formats.conf`).
4. `default`.

When a player is in several groups, the first one in `group-order` decides: `owner`, `admin`, `moderator`, `helper`,
`vip`, `default`.

## Writing colours in chat

A player's own words are plain text. With `uxmchat.format.colour` they may write `&`-colour codes, with
`uxmchat.format.hex` hex colours, with `uxmchat.format.decoration` bold, italic and the other decorations, and with
`uxmchat.format.magic` obfuscated text. All four are given to operators only.

## Small touches

| Key in `chat` | Default | Does |
|---|---|---|
| `grammar` | `false` | Starts each line with a capital letter and ends it with a full stop |
| `stretch-limit` | `3` | The most times a letter may repeat: `heyyyyyy` becomes `heyyy`; 0 leaves lines alone |
| `model` | `system` | `system`: every reader gets the line drawn by this plugin, the same for Java and Bedrock, and nobody can report it to Mojang. `signed`: Paper's chat carries the line and its signature; the report button and the "not secure" popup stay hidden |

Related: [formats.conf](../config/formats-conf.md), [Channels](channels.md)
