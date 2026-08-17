---
title: Rank and nickname sync
order: 32
description: Mapping LuckPerms groups to Discord roles, and formatting nicknames.
icon: users-round
---

Rank sync maps LuckPerms groups to Discord roles and keeps them in step. Nickname sync sets the
linked user's Discord nickname from a format string you write.

Both live in `rank-sync.yml`.

## Rank sync

```yaml
rank-sync:
  enabled: true
  sync-on-link: true
  sync-on-join: true
  sync-interval: 300
  remove-old-roles: true
```

| Key | Meaning |
|---|---|
| `sync-on-link` | Sync the moment an account is linked |
| `sync-on-join` | Sync when the player joins the server |
| `sync-interval` | Seconds between automatic sweeps; `0` disables them |
| `remove-old-roles` | Take away mapped roles the player no longer qualifies for |

`remove-old-roles: true` is what makes this a *sync* rather than a grant. With it off, a player
demoted in Minecraft keeps their Discord role forever.

It only ever removes roles that appear in `rank-mappings`. Roles the plugin does not know about are
never touched, so a manually assigned decoration role is safe.

## Mappings

```yaml
rank-mappings:
  - minecraft-group: "owner"
    discord-role-id: "OWNER_ROLE_ID"
    priority: 100
    post-link-commands:
      - "give %player% diamond 64"

  - minecraft-group: "vip"
    discord-role-id: "VIP_ROLE_ID"
    priority: 50
    post-link-commands:
      - "give %player% diamond 8"
```

A player in several groups gets every matching role — the mapping is not exclusive. `priority`
decides which group counts as their *primary* rank, which is what `%primary_rank%` resolves to in a
nickname format.

The `default` group should be mapped too, so that every linked player receives a member role.

### post-link-commands

Run once, when an account is linked, for the highest-priority group the player is in at that moment.
This is the rank-specific link reward; `linking.post-link-commands` in `config.yml` is the one
everyone gets.

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

Format placeholders:

| Placeholder | Value |
|---|---|
| `%player_name%` | Minecraft username |
| `%player_displayname%` | Minecraft display name |
| `%discord_name%` | Discord username |
| `%discord_global_name%` | Discord global name, if set |
| `%server_name%` | `plugin.server-name` |
| `%primary_rank%` | Highest-priority group from `rank-mappings` |
| `%all_ranks%` | Every group, comma separated |

Discord caps nicknames at 32 characters. `max-length` and `truncation-suffix` handle names that
exceed it, so a long username becomes `VeryLongMinecraftNa... ` rather than a rejected API call.

`reset-on-unlink: true` restores the Discord username when the accounts are separated.

<Callout type="warning" title="Two things a bot cannot do">

It cannot change the guild owner's nickname, and it cannot manage anyone whose highest role is at or
above its own. Both are Discord rules, not plugin limitations. Move the bot's role to the top of the
list, and keep `skip-on-permission-error: true` so the owner's unchangeable nickname does not spam
your console every five minutes.

</Callout>

## Forcing a sync

```
/uxmdiscordsync forcesync            # everyone online
/uxmdiscordsync forcesync <player>   # one player
```

Run this after editing `rank-mappings` rather than waiting for `sync-interval` — and use it to check
your role ids before assuming they are right, because a wrong id is indistinguishable from a
permission problem until you look.
