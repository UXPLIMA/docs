---
title: Commands
order: 20
description: The game commands from commands.yml, the admin command and the Discord slash commands.
icon: terminal
---

The game commands are not fixed: their names, aliases, permissions and cooldowns come from
`commands.yml`, and everything in the tables below is the shipped default.

## Player commands

| Command | Aliases | Permission | Cooldown | What it does |
|---|---|---|---|---|
| `/link` | `/linkaccount`, `/hesapeşle` | none | 10s | Gives a six-digit code to enter in Discord. Clicking the code copies it |
| `/unlink` | `/hesapkaldır` | none | 30s | Removes the link, if `linking.allow-unlink` is `true` |
| `/linkstatus` | `/hesapdurum` | none | 5s | Shows whether you are linked, and to which Discord account |
| `/boostprize` | `/boostödül` | `uxmdiscordsync.boostreward` | 5s | Claims the boost reward. See [Boost rewards](features/boost-rewards.md) |
| `/discordreward` | `/discordödül`, `/dcreward`, `/levelreward` | none | 5s | Claims every level reward earned and not yet taken. See [Level system](features/level-system.md) |

A cooldown starts with every use, also one that was refused.

Staff with `uxmdiscordsync.admin` can check someone else with `/linkstatus <player>`, also from the
console. The player has to be online.

## Admin commands

`/uxmdiscordsync`, or `/dsadmin` for short, needs `uxmdiscordsync.admin`. Each subcommand has its
own permission as well. Typed without a subcommand, it lists them all.

| Subcommand | What it does |
|---|---|
| `reload` | Reads every file again. Refused, with the file and line, if a file has a YAML error |
| `stats` | Linked accounts, linked players online, the Discord server's boosts, the database type and the cache size |
| `forcelink <player> <discord id> [discord name]` | Links an online player to a Discord account without a code |
| `forceunlink <player>` | Removes a player's link, also for a player who is offline |
| `forcesync <player>` | Syncs an online player's ranks to Discord now |
| `boostowner <player>` | Gives the boost rewards of the player's Discord account to this Minecraft account |
| `network` | Shows the multi-server status. See [Multi-server](features/multi-server.md#checking-the-network) |
| `debug` | Turns debug output on or off until the next reload or restart |
| `database importflatfile` | Copies old flatfile data into the SQL database. See [Database](config/database.md#moving-off-flatfile) |

### forcelink

`forcelink` refuses a Discord account that is linked to someone else. If the player is already
linked to another Discord account, that link is removed first, as a normal unlink. The new link gets
the linked roles and an audit entry, but no first-link rewards.

Write the Discord name as the last argument. Left out, the link is stored under the name
`ForceLinked`, and that is what `/linkstatus` and the logs show.

`forcelink` is the answer when a player has lost the Discord account they linked: link the player to
the new account and the old link goes with it.

### boostowner

One boost pays one Minecraft account: the first one the booster's Discord account was ever linked
to. If they move their Discord account to another Minecraft account, `/boostprize` on the new one
names the old one instead of paying. `boostowner <new account>` moves the boost, carries the
cooldown along so it cannot be claimed twice, and runs the `boost-end-commands` for the old account
to take back what it got.

## Changing a command

```yaml
commands:
  link:
    enabled: true
    aliases:
      - "link"
      - "linkaccount"
    permission: ""
    cooldown: 10
    player-only: true
```

| Key | Meaning |
|---|---|
| `enabled` | `false` removes the command |
| `aliases` | The first entry is the command's name, the rest are aliases |
| `permission` | `""` means everyone may use it |
| `cooldown` | Seconds between uses, per player. `0` for none |
| `player-only` | `true` refuses it from the console |

If another plugin already owns `/link`, rename it here rather than fighting over the name. The admin
command has a `subcommands` section where each one can be turned off or given another permission.
Changes apply on `/dsadmin reload`, and online players get the new command list at once.

## Discord slash commands

| Command | What it does | Shown when |
|---|---|---|
| `/level [user]` | A member's text and voice level | The level system is on |
| `/leaderboard <type>` | The top 10 by text or voice XP | The level system is on |
| `/servercommand <server> <command>` | Runs a console command on that server | `discord-server-command.enabled` is `true` |
| `/broadcast <server> <message>` | Sends a message to everyone on that server | `discord-broadcast.enabled` is `true` |

Plus every file in `discord-commands/`; see [Custom Discord commands](config/custom-commands.md).

The `server` option offers the servers that are running, by `plugin.server-name`. On a single
server that is its own name.

`/servercommand` and `/broadcast` only answer the Discord users listed in their `authorized-users`.
A Discord role or administrator rights do not count. `/servercommand` can also be limited to
`allowed-channels`. Both reply so that only the sender sees it, and take at most 1000 characters.

<Callout type="danger" title="/servercommand is console access">

Anyone in `discord-server-command.authorized-users` can run any command as the console, from
Discord. That is full control of the server, held by a Discord account whose security you do not
control. Leave it off unless you need it, list only people you would give the console to, and have
them turn on 2FA on their Discord accounts.

</Callout>

A `/broadcast` message can use colours, bold, italic, gradients and line breaks in MiniMessage, but
not click or hover actions. The `prefix` in front of it should say plainly that it came from Discord.
