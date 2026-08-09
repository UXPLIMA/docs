---
title: Roles & Permissions
order: 670
description: Protection in uxmSkyblock is role-based. Every team member holds a role,
  and a role is a set of permissions. If a member lacks a permission, the action is
  blocked inside the island.
---

The **owner always has every permission**, regardless of role config.

---

## Built-in Roles

Defined in [roles.yml](../configuration/roles-yml.md). You can rename them and
change their permissions freely.

| Role | Display | What they can do |
|------|---------|------------------|
| **visitor** | `&7Visitor` | Nothing — look only |
| **farmer** | `&aFarmer` | Farm and interact |
| **member** | `&fMember` | Build, break, containers, mobs, fly, bank |
| **architect** | `&bArchitect` | Member + set home & warps |
| **moderator** | `&eModerator` | Architect + invite/kick/ban, settings, upgrades, manage members |
| **owner** | `&6Owner` | Everything (`*`) |

---

## All Permissions

| Permission | Allows |
|------------|--------|
| `BLOCK_PLACE` | Place blocks |
| `BLOCK_BREAK` | Break blocks |
| `INTERACT` | Use doors, buttons, levers, etc. |
| `CONTAINER` | Open chests, furnaces, hoppers, etc. |
| `FARM` | Plant and harvest crops |
| `PICKUP_ITEMS` | Pick up dropped items |
| `DROP_ITEMS` | Drop items |
| `DAMAGE_MOBS` | Attack mobs and animals |
| `FLY` | Use `/is fly` on the island |
| `BANK` | Withdraw from the island bank |
| `INVITE` | Invite players |
| `KICK` | Remove members |
| `BAN` | Ban / unban players |
| `MANAGE_MEMBERS` | Change members' roles |
| `SET_HOME` | Set the island spawn |
| `SET_WARP` | Set / delete public warps |
| `TOGGLE_SETTINGS` | Flip island flags |
| `UPGRADE` | Buy upgrades |
| `TRANSFER` | Transfer ownership |
| `DELETE_ISLAND` | Delete the island |
| `*` | All of the above |

<Callout type="note" title="BANK is for withdrawals only">

**Any** team member can deposit into the bank. The `BANK` permission only
controls who can **withdraw**.

</Callout>

---

## Custom Roles

Owners can build their own ranks per-island at runtime, without editing any file:

```
/is role create builder
/is role perm builder BLOCK_PLACE on
/is role perm builder BLOCK_BREAK on
/is role perm builder CONTAINER on
/is role Steve builder
```

Custom roles are stored on the island itself, so different islands can have totally
different ranks. See [Team & Roles](../commands/team-roles.md) for all the role
commands.

---

## How Enforcement Works

When a player tries to do something on an island they're not the owner of, the
plugin checks their role's permission for that action. No permission → the action
is cancelled. Visitors (non-members) effectively have the **visitor** role's
permission set.
