---
title: Permissions
order: 60
description: Command nodes, the two Discord-side authorisation lists, and suggested groups.
icon: shield-check
---

uxmDiscordSync declares no permissions in `plugin.yml`. Every command permission comes from
`commands.yml`, which means the nodes below are defaults you can change — including to nothing.

## Command permissions

| Permission | Grants | Default in `commands.yml` |
|---|---|---|
| — | `/link`, `/unlink`, `/linkstatus`, `/discordreward` | No permission required |
| `uxmdiscordsync.boostreward` | `/boostprize` | Required |
| `uxmdiscordsync.2fa` | `/2fa` | Required |
| `uxmdiscordsync.admin` | `/uxmdiscordsync` | Required |
| `uxmdiscordsync.admin.reload` | `reload` | Required |
| `uxmdiscordsync.admin.stats` | `stats` | Required |
| `uxmdiscordsync.admin.forcelink` | `forcelink` | Required |
| `uxmdiscordsync.admin.forceunlink` | `forceunlink` | Required |
| `uxmdiscordsync.admin.forcesync` | `forcesync` | Required |
| `uxmdiscordsync.admin.debug` | `debug` | Required |
| `uxmdiscordsync.admin.database` | `database` | Required |

Since nothing is declared in `plugin.yml`, these nodes are unknown to Bukkit until you grant them:
operators have them implicitly, and nobody else does.

`uxmdiscordsync.boostreward` is required by default, which means a fresh install has boost rewards
that no player can claim. Grant it to your default group, or clear the `permission` field for that
command in `commands.yml`.

## Other permissions

| Permission | Effect |
|---|---|
| `uxmdiscordsync.admin.notify` | Receives 2FA denial alerts, if `on-deny.notify-admins` is on |
| `uxmdiscordsync.chatlog.ignore` | Chat is not logged, if set as `chat-logging.filter.ignored-permission` |

Both are configurable: the first is `two-factor.yml → on-deny.admin-permission` and the second is
`logging.yml → chat-logging.filter.ignored-permission`, which is empty by default. The nodes above
are the suggested values, not fixed ones.

`plugin.version-checker.notify-permission` decides who is told about a new plugin version on join —
`uxmdiscordsync.admin` by default.

## Discord-side authorisation

Two features are gated by Discord **user id**, not by permission or role:

```yaml
discord-server-command:
  authorized-users: ["123456789012345678"]

discord-broadcast:
  authorized-users: ["123456789012345678"]
```

Being an administrator in the guild grants neither. This is intentional: `/servercommand` is console
access, and it should be held by named people rather than by whoever currently has a role.

Custom Discord commands are gated per file by `required-roles` and `required-user-ids` — see
[Custom Discord commands](config/custom-commands.md#restricting-a-command).

## A working default group

```
uxmdiscordsync.boostreward
uxmdiscordsync.2fa
```

Linking, unlinking, status and level rewards need nothing, so those two are all a normal player
requires.

## A staff group

```
uxmdiscordsync.admin
uxmdiscordsync.admin.stats
uxmdiscordsync.admin.forcesync
uxmdiscordsync.admin.notify
```

Enough to diagnose and fix a sync problem. `forcelink`, `forceunlink`, `reload`, `debug` and
`database` change state or reveal connection details, and belong to a smaller group.
