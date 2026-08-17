---
title: Server stats and presence
order: 37
icon: activity
---

Two ways the bot shows what the server is doing: a live embed in a channel, and its own status
message.

## The stats embed

```yaml
server-stats:
  enabled: false
  channel-id: "YOUR_CHANNEL_ID_HERE"
  update-interval: 60
  embed:
    title: "📊 Server Status"
    color: "#00a2ff"
    footer: "Last updated"
    field-names:
      players: "👥 Players"
      tps: "⚡ TPS"
      mspt: "⏱️ MSPT"
      uptime: "🕐 Uptime"
      memory: "💾 Memory"
    show-players: true
    show-tps: true
    show-mspt: true
    show-uptime: true
    show-memory: true
```

One embed, edited in place every `update-interval` seconds rather than reposted, so the channel does
not fill up. Give it a channel of its own where nobody can post — an edited message that other
messages have scrolled past is not much use.

Each `show-` switch removes a field. TPS, MSPT and memory are useful to staff and mean nothing to
most players; a public status channel is usually better with only `show-players` and `show-uptime`.

`update-interval: 60` is a sensible floor. Editing a message costs an API call, and there is nothing
to learn from a TPS figure that refreshes every five seconds.

## Bot presence

```yaml
discord:
  presence:
    enabled: true
    update-interval: 60
    messages:
      - type: "PLAYING"
        text: "with %online_players% players"
      - type: "WATCHING"
        text: "%linked_accounts% linked accounts"
      - type: "LISTENING"
        text: "commands on %server_name%"
```

The list rotates, one entry per `update-interval`.

| Type | Reads as |
|---|---|
| `PLAYING` | Playing … |
| `WATCHING` | Watching … |
| `LISTENING` | Listening to … |
| `COMPETING` | Competing in … |
| `STREAMING` | Streaming … |

Placeholders: `%online_players%`, `%max_players%`, `%linked_accounts%`, `%server_name%`,
`%total_boosts%`.

## Voice presence

```yaml
discord:
  voice:
    enabled: false
    channel-id: ""
    auto-reconnect: true
    self-mute: true
    self-deafen: true
```

Parks the bot in a voice channel so it shows as present. `self-deafen: true` is worth keeping —
a bot that is not deafened still receives every voice packet in the channel, for no purpose.

`auto-reconnect: true` brings it back after a gateway drop.
