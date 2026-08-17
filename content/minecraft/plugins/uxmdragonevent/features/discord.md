---
title: Discord announcements
order: 38
icon: megaphone
---

The plugin can post to Discord directly — a countdown before the event, an announcement when it
starts, and a result when it ends. No second plugin is involved; the bot is embedded.

Configured in `discord.yml`, off by default.

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

`token` is a bot token from the [Discord Developer Portal](https://discord.com/developers/applications);
`channel-id` is the text channel it posts in. The bot needs **View Channel**, **Send Messages** and
**Embed Links** there.

`notify-times` are minutes before the start. Each fires the `timer` embed once.

## The three embeds

| Key | When |
|---|---|
| `timer` | At each entry in `notify-times` |
| `start` | When the event begins |
| `end` | When it finishes |

Each is the same shape:

```yaml
embeds:
  start:
    message: '<@&roleid>'
    title: "🔥 Dragon Event Is Now LIVE!"
    url: "https://..."
    description: "The Ender Dragon has woken up!"
    color: "00e5ff"
    thumbnail: "https://..."
    image: "https://..."
    author:
      name: "⚔️ Dragon Event System"
      url: "https://..."
      icon-url: "https://..."
    footer:
      text: "Powered by uxmDragonEvent"
      icon-url: "https://..."
    fields:
      field1:
        name: "⚔️ Event Status"
        value: "🟢 ACTIVE / LIVE"
        inline: true
```

| Key | Notes |
|---|---|
| `message` | Plain text above the embed — where a role ping goes |
| `color` | Hex **without** a leading `#` |
| `fields` | Named `field1`, `field2`, …; `inline: true` puts up to three side by side |

Note the colour format differs from most plugins: `"00e5ff"`, not `"#00e5ff"`.

## Pinging a role

```yaml
message: '<@&roleid>'
```

Replace `roleid` with the role's id — the full form is `<@&123456789012345678>`. Make the role
mentionable in Discord, or give the bot **Mention @everyone, @here and All Roles**.

Ping on `start` and on the last `notify-times` entry. Pinging on all four is how a role gets muted.

## Placeholders

The `end` embed can name the winner:

```yaml
fields:
  field2:
    name: "👑 Top Slayer"
    value: "🥇 %dragonevent_top_name_1%"
```

Any [placeholder](../placeholders.md) works, and `timer` supports Discord's own timestamp format:

```yaml
description: "⏰ **Time Remaining:** <t:%timestamp%:R>"
```

`%timestamp%` is the event's start time as a Unix timestamp, and `<t:...:R>` makes Discord render it
as a live relative countdown in each viewer's own timezone. That is better than a fixed time string
for exactly that reason — an international community reads one message correctly.

## Announcing without a bot

If you already run a Discord bridge, leave this off and use the `commands.start` and `commands.end`
hooks in `config.yml` to trigger whatever your other plugin uses.
