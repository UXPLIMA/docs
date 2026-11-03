---
title: Members and roles
order: 304
description: Invites, timed memberships, bans, transfers, roles, priorities and the thirty six role permissions.
icon: users
---

## Members

- `/claim invite <player> [duration]` sends an invite that lasts `members.invite-minutes` (10); the player joins with
  `/claim accept`. With `members.direct-add = true` the player joins at once. A duration such as `7d` ends the
  membership by itself.
- `/claim kick`, `/claim ban <player> [duration]` and `/claim unban` keep players out; a ban takes a member's role.
  `/claim leave` leaves a claim you are in.
- `/claim transfer <player>` gives the claim away; the old owner is not left inside.

## Roles

Every claim starts with the roles in `roles.conf`; an owner adds roles of their own in the [roles window](../menus/members.md#roles),
up to their `roles` limit, and chooses what each may do.

| Role | Priority | May |
|---|---|---|
| `visitor` | 0 | Everybody who is not a member: enter, doors, switches, pressure plates, item pickup, ender pearls, teleport |
| `member` | 10 | Every world permission |
| `moderator` | 50 | Every world permission, and invite, kick, ban, manage members, manage sub areas, set spawn |
| `owner` | 100 | Everything, whatever is written |

A higher priority outranks a lower: nobody kicks, bans or promotes a player whose role is as high as their own.
`visitor` and `owner` must stay.

## Role permissions

| Kind | Words |
|---|---|
| In the world | `enter`, `build`, `break`, `containers`, `doors`, `switches`, `pressure-plates`, `redstone`, `workstations`, `beds`, `animals`, `monsters`, `villagers`, `vehicles`, `item-frames`, `armor-stands`, `leash`, `buckets`, `fire`, `farming`, `item-pickup`, `item-drop`, `ender-pearl`, `sign-edit`, `teleport` |
| On the claim | `invite`, `kick`, `ban`, `manage-members`, `manage-roles`, `manage-flags`, `manage-rules`, `manage-chunks`, `manage-sub-areas`, `set-spawn`, `edit-info` |

`uxmsimpleclaims.admin.manage` manages every claim as if its holder owned it.

Related: [roles.conf](../config/roles-conf.md), [Member commands](../commands/members.md)
