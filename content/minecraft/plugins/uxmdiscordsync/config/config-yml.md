---
title: config.yml
order: 51
description: The plugin section, the bot, times and dates, and the Discord commands that act on the server.
icon: file-cog
---

`config.yml` holds the settings the whole plugin shares. The larger sections have pages of their own:
`database` on [Database](database.md), `multi-server` on [Multi-server](../features/multi-server.md),
`discord.linking` and `linking` on [Account linking](../features/account-linking.md),
`discord.presence`, `discord.voice` and `server-stats` on
[Server stats and presence](../features/server-stats.md), `chat-bridge` on
[Chat bridge](../features/chat-bridge.md). This page covers the rest.

## plugin

```yaml
plugin:
  license-key: ""
  debug: false
  server-name: "main"
  language: "en_US"
  version-checker:
    enabled: true
    check-interval: 3600
    notify-on-join: true
    notify-permission: "uxmdiscordsync.admin"
```

| Setting | Meaning |
|---|---|
| `license-key` | Your license key. Without a valid one the plugin does not start |
| `debug` | Detailed console output. `/dsadmin debug` switches it until the next reload or restart |
| `server-name` | This server's name: letters, digits, spaces, `-` and `_`, at most 50 characters. Read once at startup |
| `language` | The language file for messages in game: `en_US`, `tr_TR` or one you add. See [Languages](languages.md) |
| `version-checker.enabled` | Check uxplima.com for new versions and tell the console |
| `version-checker.check-interval` | Seconds between checks, at least 60. `0` turns the check off |
| `version-checker.notify-on-join` | Also tell players with `notify-permission` when they join |

The license is checked when the server starts. If it stops being valid while the server runs, the
plugin keeps running and warns; the next start needs a valid license again.

## discord

```yaml
discord:
  token: ""
  guild-id: ""
```

The bot token and the id of your Discord server, from [Creating the bot](../getting-started/discord-bot.md).
Both need a restart. `discord.webhooks` sets how the logs are sent; see
[Logging](../features/logging.md#sending-and-retrying).

## localization

```yaml
localization:
  formats:
    datetime: "yyyy-MM-dd HH:mm:ss"
  timezone: "server"
```

`%timestamp%` in the logs and the 2FA request is written in `datetime` and shown in `timezone`.

| Setting | Values |
|---|---|
| `datetime` | A Java date pattern: `dd.MM.yyyy HH:mm` gives `28.10.2026 20:00`. An invalid pattern falls back to the default |
| `timezone` | `server` for the machine's own timezone, or a region such as `Europe/Istanbul`, `Europe/London` or `UTC`. An unknown one counts as `server` |

Set the timezone your players and staff live in if the server is hosted somewhere else: a log is only
useful if everyone reads its times the same way.

## discord-server-command

```yaml
discord-server-command:
  enabled: false
  authorized-users:
    - "123456789012345678"
  allowed-channels: []
  embed: ...
```

Turns on `/servercommand` in Discord, which runs a console command on a server you pick. Read the
warning on [Commands](../commands.md#discord-slash-commands) before you do.

| Setting | Meaning |
|---|---|
| `authorized-users` | Discord user ids allowed to use it. Nobody else is, whatever their roles |
| `allowed-channels` | Channel ids it works in. Empty means every channel |
| `embed` | The confirmation. `%command%`, `%server%`, `%executor_mention%` and `%executor_id%` |

The server named in the command checks these against its own `config.yml`, so each server decides
for itself who may run commands on it.

## discord-broadcast

```yaml
discord-broadcast:
  enabled: false
  authorized-users:
    - "123456789012345678"
  prefix: "<gradient:#FFD700:#FF8C00>[Discord Broadcast]</gradient> <gray>►</gray> "
  embed: ...
```

Turns on `/broadcast`, which sends a message to every player on a server you pick.

| Setting | Meaning |
|---|---|
| `authorized-users` | Discord user ids allowed to use it |
| `prefix` | Put in front of the message in game, in MiniMessage |
| `embed` | The confirmation. `%message%`, `%sender%`, `%sender_mention%` and `%sender_id%` |

The prefix should say plainly that the message comes from Discord. A broadcast that looks like a
server announcement lets one stolen Discord account cause a lot of confusion.

## config-version

Do not change it. The plugin writes its own version here and uses it to update your files.
