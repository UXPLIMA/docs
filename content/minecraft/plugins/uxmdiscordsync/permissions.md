---
title: Permissions
order: 60
description: The permission nodes, who has them by default, and the Discord-side lists that permissions do not cover.
icon: shield-check
---

Every node below is declared by the plugin with a default, so operators have the staff nodes and
every player has `uxmdiscordsync.boostreward` without any setup. The command nodes are the ones
`commands.yml` ships with: if you rename one there, grant the new name instead.

## Nodes

| Permission | Default | Grants |
|---|---|---|
| `uxmdiscordsync.boostreward` | Everyone | `/boostprize` |
| `uxmdiscordsync.admin` | Operators | `/dsadmin`, `/linkstatus <player>`, and the in-game notice of a new version |
| `uxmdiscordsync.admin.reload` | Operators | `reload` |
| `uxmdiscordsync.admin.stats` | Operators | `stats` |
| `uxmdiscordsync.admin.forcelink` | Operators | `forcelink` |
| `uxmdiscordsync.admin.forceunlink` | Operators | `forceunlink` |
| `uxmdiscordsync.admin.forcesync` | Operators | `forcesync` |
| `uxmdiscordsync.admin.boostowner` | Operators | `boostowner` |
| `uxmdiscordsync.admin.network` | Operators | `network` |
| `uxmdiscordsync.admin.debug` | Operators | `debug` |
| `uxmdiscordsync.admin.database` | Operators | `database importflatfile` |
| `uxmdiscordsync.admin.notify` | Operators | Hears about a denied 2FA login. See [Two-factor login](features/two-factor.md#denied-and-timed-out) |
| `uxmdiscordsync.2fa.required` | Nobody | Is asked for 2FA, when `two-factor.require-permission` is `true` |
| `uxmdiscordsync.admin.*` | Operators | Every `uxmdiscordsync.admin` node above |
| `uxmdiscordsync.*` | Operators | Everything except `uxmdiscordsync.2fa.required` |

`/link`, `/unlink`, `/linkstatus` and `/discordreward` need no permission.

`uxmdiscordsync.*` leaves out `uxmdiscordsync.2fa.required` on purpose: with
`require-permission: true`, a wildcard would otherwise put every operator under 2FA without anyone
deciding it. Grant the node itself to whoever should be asked.

## Nodes you name yourself

Three settings take a permission of your choosing:

| Setting | Effect |
|---|---|
| `two-factor.on-deny.admin-permission` in `two-factor.yml` | Who hears about a denied 2FA login. Ships as `uxmdiscordsync.admin.notify` |
| `plugin.version-checker.notify-permission` in `config.yml` | Who is told about a new version on join. Ships as `uxmdiscordsync.admin` |
| `chat-logging.filter.ignored-permission` in `logging.yml` | Players holding it are left out of the chat log. Ships empty, which turns it off |

## Permissions Discord decides

Some things are decided by Discord ids, not by permissions:

| Who | Set in |
|---|---|
| Who may use `/servercommand` | `discord-server-command.authorized-users` in `config.yml` |
| Who may use `/broadcast` | `discord-broadcast.authorized-users` in `config.yml` |
| Who may use a custom command | `required-roles` and `required-user-ids` in its file. See [Custom Discord commands](config/custom-commands.md#who-may-use-it) |

Administrator rights in Discord grant none of these. `/servercommand` is console access, and it
should belong to named people rather than to whoever holds a role this week.

## A staff group

```
uxmdiscordsync.admin
uxmdiscordsync.admin.stats
uxmdiscordsync.admin.forcesync
uxmdiscordsync.admin.network
uxmdiscordsync.admin.notify
```

Enough to look into a sync problem. `forcelink`, `forceunlink`, `boostowner`, `reload`, `debug` and
`database` change data or settings, and belong to a smaller group.
