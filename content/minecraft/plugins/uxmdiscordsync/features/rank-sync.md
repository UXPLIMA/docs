---
title: Rank and nickname sync
order: 32
description: LuckPerms groups as Discord roles, rank rewards for linking, and the Discord nickname format.
icon: users-round
---

Rank sync gives a linked member the Discord roles of their LuckPerms groups and takes them away again
when the groups change. Nickname sync sets their Discord nickname from a format you write. Both are
in `rank-sync.yml`, and each has its own switch.

## Rank sync

Needs LuckPerms, and the bot needs **Manage Roles** with its own role above every mapped role.

```yaml
rank-sync:
  enabled: true
  sync-on-link: true
  sync-on-join: true
  sync-interval: 300
  remove-old-roles: true
```

| Setting | Meaning |
|---|---|
| `sync-on-link` | Sync the moment an account is linked |
| `sync-on-join` | Sync when the player joins |
| `sync-interval` | Seconds between syncs of every online player. `0` turns them off |
| `remove-old-roles` | Take away mapped roles of groups the player is no longer in |

Only players who are online are synced. A player demoted while offline keeps their role until they
next join.

`remove-old-roles` only ever removes roles that appear in `rank-mappings`. A role you gave by hand
and did not map is never touched. With `remove-old-roles: false`, the plugin only adds roles, and a
demoted player keeps theirs.

## Mappings

```yaml
  rank-mappings:
    - minecraft-group: "vip"
      discord-role-id: "123456789012345678"
      priority: 50
      post-link-commands:
        - "give %player% diamond 8"

    - minecraft-group: "default"
      discord-role-id: "234567890123456789"
      priority: 1
      post-link-commands: []
```

| Key | Meaning |
|---|---|
| `minecraft-group` | The LuckPerms group name |
| `discord-role-id` | The role it gives |
| `priority` | Higher wins where one rank has to be picked: the rank rewards and `%primary_rank%` |
| `post-link-commands` | Run once, on the player's first link, for the highest-priority rank they have |

A player in several mapped groups gets every one of their roles. Two groups may share a role: it
stays as long as the player is in either group.

The plugin reads the groups the player is in directly, as `/lp user <name> parent info` lists them.
A group the player only gets because another group inherits it does not count. Players who were
given a rank with `parent set` are no longer in `default`, so map every rank you want a role for.

The rank rewards run after `linking.post-link-commands` in `config.yml`, which everyone gets. With
the example above, a VIP gets 8 diamonds for linking and a default player nothing extra.

## What you see

After a sync that changed something, the console shows `Rank sync completed for`, the player and the
roles added and removed, and the `rank-sync` audit log records it. A sync that changed nothing is
silent.

If Discord refuses a role, the console names it once and says to move the bot's role above it; the
other roles still go through. The warning comes back after `/dsadmin reload`.

`/dsadmin forcesync <player>` syncs one online player now. Use it after editing the mappings, and to
test a new role id before trusting it.

## Nickname sync

```yaml
  nickname-sync:
    enabled: true
    sync-on-link: true
    sync-on-join: true
    sync-interval: 300
    format: "%player_name% (@%server_name%)"
    reset-on-unlink: true
    skip-on-permission-error: true
    max-length: 32
    truncation-suffix: "..."
```

Needs **Manage Nicknames**. The nickname is also updated after every rank change, because the
format can contain the rank.

| Placeholder | Value |
|---|---|
| `%player_name%` | Minecraft name |
| `%player_displayname%` | Minecraft display name |
| `%discord_name%` | Discord username |
| `%discord_global_name%` | Discord display name, or the username if none is set |
| `%server_name%` | `plugin.server-name` |
| `%primary_rank%` | The highest-priority mapped group the player is in, or one of their groups if none is mapped |
| `%all_ranks%` | Every group the player is in, separated by commas |

Without LuckPerms, both rank placeholders are empty.

| Setting | Meaning |
|---|---|
| `max-length` | Discord allows 32 characters. A longer nickname is cut and ends in `truncation-suffix` |
| `reset-on-unlink` | Remove the nickname on unlinking, so the member's own name shows again |
| `skip-on-permission-error` | Skip members the bot may not rename without a console warning |

Discord never lets a bot rename the server owner or a member whose highest role is at or above the
bot's. With `skip-on-permission-error: true` those members are skipped quietly instead of filling
the console every five minutes.

## Several servers

Each server syncs the players who are on it, from its own LuckPerms data. With
`remove-old-roles: true`, two servers with different mappings for one role undo each other's
changes. Keep `rank-mappings` the same everywhere, or share LuckPerms' database.

Related: [Account linking](account-linking.md), [Logging](logging.md#audit-log)
