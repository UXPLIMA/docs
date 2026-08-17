---
title: Chat bridge
order: 36
icon: messages-square
---

The chat bridge carries messages both ways between one Discord channel and Minecraft chat.

```yaml
chat-bridge:
  enabled: false
  discord-channel-id: "YOUR_CHANNEL_ID_HERE"
  minecraft-format: "**{player}**: {message}"
  discord-format: "<gradient:#5865F2:#7289DA>[Discord]</gradient> <white>{user}</white><dark_gray>:</dark_gray> <gray>{message}</gray>"
```

| Key | Meaning |
|---|---|
| `discord-channel-id` | The one channel that is bridged |
| `minecraft-format` | Minecraft → Discord, Markdown, with `{player}` and `{message}` |
| `discord-format` | Discord → Minecraft, MiniMessage, with `{user}` and `{message}` |

Note the placeholder style: the bridge uses `{braces}`, while most of the plugin uses `%percent%`.

Reading from Discord requires the **Message Content** intent.

## Bridge and chat log are different features

The bridge is two-way and conversational; the [chat log](logging.md) is one-way and archival. They
can point at the same channel but usually should not — a bridged channel where every Minecraft
message also arrives as a webhook post shows each message twice.

Pick one per channel: a `#minecraft-chat` that is bridged, or a `#chat-log` that is logged.

## What comes across

Only chat. Commands, join and leave messages, and death messages are not the bridge's job — join and
leave have [their own logging section](logging.md#join-and-leave), and commands are deliberately not
forwarded.

<Callout type="warning" title="The bridge is a route into your server's chat">

Anyone who can type in the bridged channel can put text in front of every player on the server,
under a name you do not control. Restrict the channel to linked members, keep it out of any
publicly-joinable category, and remember that Discord moderation is now part of your Minecraft chat
moderation.

</Callout>

MiniMessage in `discord-format` is applied to your format string, not to the user's message —
`{message}` is inserted as text, so a Discord user cannot inject colour codes or click events into
Minecraft chat.
