---
title: Discord announcements
order: 38
description: The built-in bot, the countdown, start and end messages, and every key an embed takes.
icon: megaphone
---

The plugin can post to Discord by itself: a countdown before a scheduled event, a message when an
event starts, and one when it ends. The bot is built in; no second plugin is involved.

It is set up in `discord.yml` and off by default.

```yaml
enabled: false
token: ''
channel-id: ''
notify-times:
  - 30
  - 15
  - 5
  - 1
```

## Setting up the bot

1. Create an application in the [Discord Developer Portal](https://discord.com/developers/applications),
   add a bot to it, and copy the bot's token into `token`.
2. Invite the bot to your Discord server.
3. Copy the id of the channel it should post in into `channel-id`. A text channel, an announcement
   channel or a thread all work.
4. Give the bot **View Channel**, **Send Messages** and **Embed Links** in that channel.
5. Set `enabled: true` and run `/dragon reload bot`.

`/dragon reload` alone reads the messages again but does not reconnect the bot. After changing the
token or `enabled`, use `/dragon reload bot`.

## The three messages

| Key | Sent |
|---|---|
| `timer` | Before a scheduled event, once for each entry in `notify-times` |
| `start` | When any event starts, scheduled or by `/dragon start` |
| `end` | When any event ends, including when time runs out or staff stop it |

`notify-times` are minutes before the start. An empty list uses 30, 15, 5 and 1. Events started with
`/dragon start` get no countdown.

## What an embed takes

```yaml
embeds:
  start:
    message: '<@&123456789012345678>'
    title: "Dragon Event Is Now LIVE!"
    url: "https://example.com"
    description: "The Ender Dragon has woken up!"
    color: "00e5ff"
    thumbnail: "https://example.com/thumb.png"
    image: "https://example.com/banner.png"
    author:
      name: "Dragon Event"
      url: "https://example.com"
      icon-url: "https://example.com/icon.png"
    footer:
      text: "Powered by uxmDragonEvent"
      icon-url: "https://example.com/icon.png"
    fields:
      status:
        name: "Event Status"
        value: "ACTIVE"
        inline: true
```

| Key | Notes |
|---|---|
| `message` | Plain text sent with the embed. This is where a role ping goes |
| `title`, `url` | The title, and the link it opens |
| `description` | The main text. Discord's own formatting works: `**bold**`, `\n` for a new line |
| `color` | Hex, with or without `#`: `"00e5ff"` and `"#00e5ff"` are the same |
| `thumbnail`, `image` | The small picture at the top right, and the large one at the bottom |
| `author` | A section with `name`, `url` and `icon-url`, or just a name: `author: "Dragon Event"` |
| `footer` | A section with `text` and `icon-url`, or just the text: `footer: "Powered by uxmDragonEvent"` |
| `timestamp` | A fixed date shown beside the footer, such as `2026-10-28T20:00:00+03:00` |
| `fields` | Any number of fields under names of your choice. `inline: true` puts up to three side by side |

Every link and picture must start with `https://`, `http://` or `attachment:`. Anything else is left
out of the embed without a warning.

A key you do not want can be removed. An embed needs at least a title, a description or a field to be
sent.

## Placeholders

Server-wide [placeholders](../placeholders.md) work in every text. The `end` embed can name the
winner:

```yaml
fields:
  winner:
    name: "Top Slayer"
    value: "%dragonevent_top_name_1%"
```

The `timer` embed also takes `%timestamp%`, the scheduled start time in Discord's format:

```yaml
description: "Starts <t:%timestamp%:R>, at <t:%timestamp%:t>"
```

`<t:...:R>` shows a live countdown and `<t:...:t>` the start time, both in each reader's own
timezone, so an international community reads one message correctly.

## Pinging a role

```yaml
message: '<@&123456789012345678>'
```

Use the role's id. Make the role mentionable in Discord, or give the bot **Mention @everyone, @here
and All Roles**.

Ping on `start` and at most once in the countdown: a role pinged five times per event is muted
within a week.

## Keep the text in step with the config

The default embeds describe the default event: `500 Coins to enter`, `2 Dragons` and a link to the
old wiki. They do not read `config.yml`, so change them when you change the price, the kills or the
rewards.

## When nothing is posted

The bot reports its problems only with debug output on. Add `debug: true` to the top of `config.yml`,
run `/dragon reload`, and start an event: the console then says whether the bot logged in, whether it
found the channel, and why a message failed. See [Troubleshooting](../troubleshooting.md#discord).

## Announcing without the bot

If you already run a Discord bridge plugin, leave this off and use `commands.start` and
`commands.end` in `config.yml` to trigger whatever that plugin uses. See
[Presentation](presentation.md#commands-on-start-and-end).
