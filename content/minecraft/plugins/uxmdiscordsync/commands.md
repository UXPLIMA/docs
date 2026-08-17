---
title: Commands
order: 20
description: Minecraft commands from commands.yml, and the Discord slash commands.
icon: terminal
---

Minecraft commands are **not** declared in `plugin.yml`: they are registered from `commands.yml` at
startup. Every name, alias, permission and cooldown on this page is something you can change.

## Player commands

| Command | Aliases | Permission | Cooldown |
|---|---|---|---|
| `/link` | `/linkaccount`, `/hesapeşle` | none | 10s |
| `/unlink` | `/hesapkaldır` | none | 30s |
| `/linkstatus` | `/hesapdurum` | none | 5s |
| `/boostprize` | `/boostödül` | `uxmdiscordsync.boostreward` | 5s |
| `/discordreward` | `/discordödül`, `/dcreward`, `/levelreward` | none | 5s |
| `/2fa` | n/a | `uxmdiscordsync.2fa` | 5s |

`/link` returns a six-digit code to enter in Discord. `/linkstatus` reports whether the player is
linked and to whom; it is the only player command that also works from console.

`/unlink` is available only while `linking.allow-unlink` is `true`. It runs `post-unlink-commands`
and, if `nickname-sync.reset-on-unlink` is on, puts the Discord nickname back.

`/discordreward` claims level rewards the player has earned but not yet taken; `/boostprize` claims
the boost reward, subject to `boost-rewards.yml → cooldown`.

## Admin commands

`/uxmdiscordsync`, permission `uxmdiscordsync.admin`.

| Subcommand | Permission | What it does |
|---|---|---|
| `reload` | `uxmdiscordsync.admin.reload` | Re-reads the configuration files |
| `stats` | `uxmdiscordsync.admin.stats` | Link counts and plugin statistics |
| `forcelink <player> <discord id>` | `uxmdiscordsync.admin.forcelink` | Creates a link without a code |
| `forceunlink <player>` | `uxmdiscordsync.admin.forceunlink` | Removes a link |
| `forcesync [player]` | `uxmdiscordsync.admin.forcesync` | Runs rank sync now |
| `debug` | `uxmdiscordsync.admin.debug` | Toggles debug logging |
| `database` | `uxmdiscordsync.admin.database` | Database type, size and connection state |

`forcesync` without a player syncs everyone online: the command to run after editing
`rank-mappings` rather than waiting for `sync-interval`.

## Changing command names

```yaml
commands:
  link:
    enabled: true
    aliases:
      - "link"
      - "linkaccount"
      - "hesapeşle"
    permission: ""
    cooldown: 10
    player-only: true
```

The **first** alias is the real command name; the rest are aliases. `enabled: false` unregisters the
command entirely, which is how you resolve a clash with another plugin that already owns `/link`:
rename it here rather than fighting over the name.

`permission: ""` means no permission is required. `player-only: true` refuses the command from
console.

Command changes need a restart, not a reload: commands are registered with the server at startup.

## Discord slash commands

Registered in your guild at startup.

| Command | What it does | Gated by |
|---|---|---|
| `/level [user]` | A user's level and XP | Level system enabled |
| `/leaderboard <type>` | The level leaderboard; type is `text` or `voice` | Level system enabled |
| `/broadcast <message>` | Broadcasts to the Minecraft server | `discord-broadcast.authorized-users` |
| `/servercommand <server> <command>` | Runs a command on the server | `discord-server-command.authorized-users` |

Plus every custom command in `discord-commands/`: see [Custom Discord commands](config/custom-commands.md).

<Callout type="danger" title="/servercommand is console access">

Anyone in `discord-server-command.authorized-users` can run any command as console, from Discord.
That is full control of the server, granted to a Discord account whose security you do not manage.
Leave `discord-server-command.enabled: false` unless you need it, list individual user ids rather
than everyone with a role, and require 2FA on those Discord accounts.

</Callout>

The two authorised-user lists are user ids, not roles, and they are checked before anything else:
a slash command from anyone else is refused whatever their Discord permissions are.
