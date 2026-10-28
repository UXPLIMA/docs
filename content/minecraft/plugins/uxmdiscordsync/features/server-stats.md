---
title: Server stats and presence
order: 37
description: The live stats message, the bot's rotating status and parking the bot in a voice channel.
icon: activity
---

Three ways the bot shows what the server is doing: a stats message it keeps up to date in a channel,
the status line under its name, and its presence in a voice channel. All three are in `config.yml`.

## The stats message

```yaml
server-stats:
  enabled: false
  channel-id: "123456789012345678"
  update-interval: 60
  mode: "per-server"
  embed:
    title: "%server_name%"
    color: "#00a2ff"
    footer-text: "Last updated"
    timestamp: true
    fields: []
    field-names:
      players: "Players"
      tps: "TPS"
      mspt: "MSPT"
      uptime: "Uptime"
      memory: "Memory"
    show-players: true
    show-tps: true
    show-mspt: true
    show-uptime: true
    show-memory: true
```

The bot posts one message and edits it every `update-interval` seconds instead of posting new ones.
It remembers the message across restarts. `0` turns the stats off.

| Field | Shows |
|---|---|
| Players | Online and maximum players |
| TPS | Ticks per second, marked green from 19.5, yellow from 17 and red below |
| MSPT | Milliseconds per tick |
| Uptime | Time since the server started |
| Memory | Memory in use and the maximum, in MB |

Each `show-` setting removes one field, and `field-names` renames them. The other `embed` keys are
the ones from [Concepts](../getting-started/concepts.md#embeds), and the title, description and your
own `fields` take `%server_name%`, `%online_players%` and `%max_players%`. Your own fields come first.

TPS, MSPT and memory interest staff, not players. A public status channel is usually better with only
players and uptime.

Give the stats a channel of their own where members cannot write: a message that has scrolled out of
sight is no use. In `per-server` mode, turning the stats off with a reload deletes the message. When
the server stops, the
message stays as it was; the time next to its footer shows when it was last updated.

### With several servers

| `mode` | Result |
|---|---|
| `per-server` | Every server keeps its own message. Give each one its own channel, or they stack up in one |
| `network` | One message for the whole network, with the total player count and one block per server. Every server must have the same `server-stats` settings |

In `network` mode `%server_name%` is the network name, and a server that stops leaves the message
within a few updates.

## The bot's status

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
```

The bot shows one entry at a time and moves to the next every `update-interval` seconds. `0` keeps
the first one.

| `type` | Discord shows |
|---|---|
| `PLAYING` | Playing ... |
| `WATCHING` | Watching ... |
| `LISTENING` | Listening to ... |
| `COMPETING` | Competing in ... |
| `STREAMING` | Streaming ..., with a link to Twitch |

Placeholders: `%online_players%`, `%max_players%`, `%linked_accounts%`, `%server_name%` and
`%total_boosts%`. With several servers, the online count is the whole network's and
`%server_name%` the network name.

## The voice channel

```yaml
discord:
  voice:
    enabled: false
    channel-id: "123456789012345678"
    auto-reconnect: true
    self-mute: true
    self-deafen: true
```

Parks the bot in a voice channel, where members see it in the list. It plays nothing. Keep
`self-deafen: true`: the bot has no use for the sound in the channel. The bot needs **Connect** there.

With several servers, only one of them runs the status and the voice channel. See
[Multi-server](multi-server.md#who-does-what).

Related: [Chat bridge](chat-bridge.md), [Placeholders](../placeholders.md)
