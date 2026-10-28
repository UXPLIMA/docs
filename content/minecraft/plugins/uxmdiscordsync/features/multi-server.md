---
title: Multi-server
order: 39
description: Running one bot on several servers with Redis and a shared database, and which server does what.
icon: network
---

Discord sends every button press, slash command and message to every server that runs the bot. When
several servers use the same bot token, they have to agree who answers, or the same click is
answered twice, rewards run twice, and members see `This interaction failed`. Multi-server mode makes
them agree through Redis, so each Discord action is handled once, by the right server.

If every server has a bot of its own, you do not need this mode.

## Requirements

| Requirement | If it is missing |
|---|---|
| Redis 5.0 or newer, reachable from every server | Discord actions are not handled while Redis cannot be reached |
| One MySQL, MariaDB or PostgreSQL database shared by every server | The plugin does not start |
| A different `plugin.server-name` on every server, not `main` and not `all` | The plugin does not start, or the second server with a name stays passive |
| The same bot token, the same `multi-server.secret` and the same plugin version everywhere | Messages between servers are refused; a server older than another does not start |

Set `maxmemory-policy noeviction` in Redis. With another policy a full Redis can delete what the
servers rely on, and the console warns about it at startup.

## Settings

In `config.yml`, on every server. Changes need a restart.

```yaml
plugin:
  server-name: "survival"

database:
  type: "mysql"

multi-server:
  enabled: true
  secret: ""
  network-name: "Network"
  redis:
    host: "10.0.0.5"
    port: 6379
    username: ""
    password: "..."
    database: 0
    ssl: false
    key-prefix: "uxmds"
  reward-mode: "per-server"
```

| Setting | Meaning |
|---|---|
| `secret` | Encrypts and signs what the servers send each other. Fill in 32 or more random characters, the same everywhere |
| `network-name` | Shown where the whole network is meant: the link message, the bot's status, the network stats |
| `redis.username` | For a Redis 6 user with limited rights; leave it empty on Redis 5. The user needs the prefix for keys and channels, such as `~uxmds:* &uxmds:* +@all` |
| `redis.key-prefix` | The same on every server of one network. A second network on the same Redis needs another prefix |
| `reward-mode` | `per-server` or `global` boost cooldown. See [Boost rewards](boost-rewards.md#the-cooldown) |

With `secret` empty, the bot token is used in its place, and the console says so: then anyone with
the token and access to Redis could send your servers console commands.

<Callout type="danger" title="Keep Redis private">

Bind Redis to your internal network and give it a password. A stranger who can write to it cannot
forge a command while the secret holds, but can still disrupt the network: block servers from
answering, or take the leader's place.

</Callout>

## Who does what

| Discord action | Handled by |
|---|---|
| The link button and the code | The first server to take it. Rewards and syncing run on the server the player is on, or when they next join |
| 2FA buttons | The server the player is on |
| `/servercommand`, `/broadcast` | The server named in the command, which checks its own settings |
| `/level`, `/leaderboard` | One server, from the shared database |
| Custom commands | One server, or the one named in the file's `server` key |
| Text XP | One server per message |
| Chat bridge | Every server: each sends its own chat and shows Discord messages to its own players |

One server is chosen as the leader, automatically. Only the leader:

- registers the slash commands, which are all commands of every server together
- keeps the link message up to date
- sets the bot's status, with the player count of the whole network
- joins the voice channel and counts voice XP
- keeps the stats message in `network` mode
- logs Discord members joining and leaving

If the leader stops, or loses its connection to Discord for a minute, another server takes over within
about 15 seconds. The slash commands of a server that has been off for 24 hours are removed, and come
back when it starts again.

## Checking the network

`/dsadmin network` shows:

| Line | Meaning |
|---|---|
| Mode and connection | Whether multi-server mode is on and Redis answers |
| Role | `leader`, `member`, `passive` (another running server has this name) or offline |
| Leader | Which server leads now |
| Servers | Every running server and its player count |

A quick restart of the same server is not mistaken for a second one: its old entry expires within 15
seconds.

## When Redis is down

The servers stop handling Discord actions until Redis is back, so nothing runs twice. The console
warns at most once a minute, and everything resumes by itself. Meanwhile, players asked for 2FA are
kicked with a message to try again, because the answer could not come back.

## Keep these the same everywhere

- `rank-mappings`: each server syncs the players on it. With `remove-old-roles`, different mappings
  undo each other.
- Slash command files: if two servers define one command differently, the leader's version wins and
  the console warns.
- `voice-level.xp-interval`, and the `server-stats` settings in `network` mode.

Link codes and cooldowns follow the database's clock, so a server whose own clock drifted does not
shorten them. Messages between servers that seem more than five minutes old are ignored, with a
warning to check the server clocks.

## With a proxy

uxmDiscordSync runs on the Paper servers behind BungeeCord or Velocity, not on the proxy. Install it on
every server that needs Discord features, point them at one database and one Redis, and give each its
own `server-name`.

Related: [Database](../config/database.md), [Server stats and presence](server-stats.md)
