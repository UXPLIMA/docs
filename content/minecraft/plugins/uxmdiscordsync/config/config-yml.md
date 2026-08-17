---
title: config.yml
order: 51
description: The bot, multi-server, linking, localization and the embed palette.
icon: file-cog
---

## plugin

```yaml
plugin:
  license-key: ""
  debug: false
  server-name: "main"
  language: "en_US"
  auto-save-interval: 300
  version-checker:
    enabled: true
    check-interval: 3600
    notify-on-join: true
    notify-permission: "uxmdiscordsync.admin"
```

`server-name` must be unique per server on a network — it is the identity everything multi-server
keys off. `auto-save-interval: 0` disables periodic saving, which is not recommended: data is then
only written on shutdown, and a crash loses everything since the last restart.

`language` here and `localization.language` further down are both read; keep them the same.

## discord

Bot token, guild id, presence, voice, linking and webhook rate limiting. Covered on:

- [Creating the bot](../getting-started/discord-bot.md) — token, guild, intents
- [Account linking](../features/account-linking.md) — the `linking` section
- [Server stats and presence](../features/server-stats.md) — `presence` and `voice`

```yaml
discord:
  webhooks:
    rate-limit-delay: 2000
    max-retries: 3
    retry-delay: 5000
```

`rate-limit-delay` is milliseconds between webhook posts. 2000 keeps you inside Discord's 30-per-
minute limit with room to spare. Lower it and Discord starts returning 429s, which the plugin then
has to retry — slower overall than waiting in the first place.

## multi-server

```yaml
multi-server:
  enabled: false
  link-mode: "shared"
  reward-mode: "per-server"
```

Explained in [Concepts](../getting-started/concepts.md#link-mode-and-reward-mode). Requires a shared
database — see [Database](database.md).

## linking

```yaml
linking:
  post-link-commands:
    - "give %player% diamond 1"
  allow-unlink: true
  post-unlink-commands:
    - "tellraw %player% {\"text\":\"Your account has been unlinked.\"}"
```

The commands every linked player gets. Rank-specific ones live in `rank-sync.yml`.

## localization

```yaml
localization:
  language: "en_US"
  formats:
    date: "yyyy-MM-dd"
    time: "HH:mm:ss"
    datetime: "yyyy-MM-dd HH:mm:ss"
  timezone: "server"
```

Java `SimpleDateFormat` patterns. `timezone` takes `server` or a zone id such as `UTC`,
`Europe/Istanbul` or `America/New_York`.

Set it explicitly rather than leaving `server` if your host is in a different timezone from your
players — timestamps in audit logs are only useful if everyone reads them the same way.

## discord-server-command

```yaml
discord-server-command:
  enabled: false
  authorized-users:
    - "123456789012345678"
```

Console access from Discord. See the warning on [Commands](../commands.md#discord-slash-commands).

## discord-broadcast

```yaml
discord-broadcast:
  enabled: false
  authorized-users:
    - "123456789012345678"
  prefix: "<gradient:#FFD700:#FF8C00>[Discord Broadcast]</gradient> <gray>►</gray> "
```

`/broadcast` sends a message to every player. The prefix is MiniMessage and should say plainly that
the message came from Discord — a broadcast that looks like a server announcement is a good way for
one compromised Discord account to cause a lot of confusion.

## server-stats and chat-bridge

Documented on [Server stats](../features/server-stats.md) and
[Chat bridge](../features/chat-bridge.md).

## embeds

```yaml
embeds:
  colors:
    success: "#57F287"
    error: "#ED4245"
    warning: "#FEE75C"
    info: "#5865F2"
    boost: "#EB459E"
    link: "#00CED1"
    unlink: "#FFA500"
    rank-sync: "#9B59B6"
```

The shared palette. Individual embeds can override their own colour; these are the defaults, and
setting them once is how the whole bot ends up looking like it belongs to your server.

## config-version

```yaml
config-version: 1
```

Used for automatic migration between plugin versions. Do not edit it.
