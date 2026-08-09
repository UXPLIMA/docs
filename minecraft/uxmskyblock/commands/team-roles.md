---
title: Team & Roles
order: 530
description: Everything for building and running your island team — members, invites,
  bans, and roles.
---

---

## Managing Members

### `/is invite <player>`

Sends an invitation. The invite expires after `command.invite-expire-seconds`
(120s by default). Requires the `INVITE` permission.

```
/is invite Steve
```

### `/is accept` / `/is deny`

The invited player accepts (`accept`, `kabul`) or rejects (`deny`, `reddet`, `ret`)
a pending invite. On accept they join with the default **member** role.

### `/is leave`

Leave the island team. Owners can't leave — transfer or delete the island instead.

<Callout type="info" title="Leaving and inventories">

If `island.clear-inventory-on-leave` is enabled in config, a member's inventory
is cleared when they leave or are kicked, so they can't walk off with island loot.

</Callout>

### `/is kick <player>`

Removes a member. Requires the `KICK` permission. Aliases: `at`, `çıkar`.

### `/is members`

Lists every member and their role.

---

## Bans

### `/is ban <player>`

Bans a player from the island — they can't visit or be invited. Requires the `BAN`
permission.

### `/is unban <player>`

Lifts a ban.

---

## Trust

### `/is trust <player>` / `/is untrust <player>`

A quick way to give or revoke trusted access without managing full roles.

---

## Ownership

### `/is transfer <player>`

Hands island ownership to another member. The old owner becomes a regular member.
Requires the `TRANSFER` permission (owner only by default).

```
/is transfer Steve
```

---

## Roles

Every member has a **role** — a bundle of permissions. Built-in roles live in
[roles.yml](../configuration/roles-yml.md); owners can also create custom roles on
the fly.

### `/is role <player> <role>`

Assigns a built-in or custom role to a member.

```
/is role Steve architect
/is role Alex builder        # a custom role you made
```

### `/is role create <id>`

Creates a new custom role for **this island**, starting with no permissions.

```
/is role create builder
```

### `/is role perm <id> <permission> <on|off>`

Grants or removes a single permission on a custom role.

```
/is role perm builder BLOCK_PLACE on
/is role perm builder CONTAINER on
```

### `/is role delete <id>`

Deletes a custom role. Members holding it fall back to the default role.

### `/is role list`

Lists every role on the island (built-in + custom) with their permissions.

---

## Role Permissions

| Permission | Allows |
|------------|--------|
| `BLOCK_PLACE` / `BLOCK_BREAK` | Build / mine |
| `INTERACT` | Use doors, buttons, levers |
| `CONTAINER` | Open chests, furnaces, etc. |
| `FARM` | Harvest and plant crops |
| `PICKUP_ITEMS` / `DROP_ITEMS` | Pick up / drop items |
| `DAMAGE_MOBS` | Hit mobs and animals |
| `FLY` | Use `/is fly` |
| `BANK` | Withdraw from the island bank |
| `INVITE` / `KICK` / `BAN` | Manage who's on the team |
| `MANAGE_MEMBERS` | Change member roles |
| `SET_HOME` / `SET_WARP` | Set island spawn / warps |
| `TOGGLE_SETTINGS` | Flip flags in `/is settings` |
| `UPGRADE` | Buy upgrades |
| `TRANSFER` / `DELETE_ISLAND` | Transfer / delete the island |
| `*` | Everything (owner) |

See [Roles & Permissions](../protection/roles-permissions.md) for the full breakdown.
