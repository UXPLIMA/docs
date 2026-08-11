---
title: roles.yml
order: 750
description: roles.yml defines the built-in island roles and the permissions each
  one grants. The owner always has every permission, no matter what's here.
---

For the gameplay meaning of roles and the full permission reference, see
[Roles & Permissions](../protection/roles-permissions.md).

---

## Format

```yaml
roles:
  member:
    name: "&fMember"
    permissions:
      - BLOCK_PLACE
      - BLOCK_BREAK
      - INTERACT
      - CONTAINER
      - FARM
      - PICKUP_ITEMS
      - DROP_ITEMS
      - DAMAGE_MOBS
      - FLY
      - BANK
```

| Field | Meaning |
|-------|---------|
| `name` | Display name (color codes allowed) |
| `permissions` | List of permission nodes, or `["*"]` for all |

---

## The Default Roles

| Role | Permissions |
|------|-------------|
| `visitor` | *(none)* |
| `farmer` | `FARM`, `INTERACT` |
| `member` | build, break, interact, container, farm, pickup, drop, damage mobs, fly, bank |
| `architect` | member + `SET_HOME`, `SET_WARP` |
| `moderator` | architect + `INVITE`, `KICK`, `BAN`, `TOGGLE_SETTINGS`, `UPGRADE`, `MANAGE_MEMBERS` |
| `owner` | `*` (everything) |

---

## All Permission Nodes

```
BLOCK_PLACE, BLOCK_BREAK, INTERACT, CONTAINER, FARM,
PICKUP_ITEMS, DROP_ITEMS, DAMAGE_MOBS, INVITE, KICK, BAN,
SET_HOME, SET_WARP, TOGGLE_SETTINGS, UPGRADE, MANAGE_MEMBERS,
FLY, BANK, DELETE_ISLAND, TRANSFER       ('*' = all)
```

<Callout type="note" title="BANK is withdraw-only">

Anyone on the team can **deposit** into the bank. `BANK` controls who can
**withdraw**.

</Callout>

---

## Built-in vs. Custom Roles

`roles.yml` seeds the **built-in** roles every island starts with. Owners can also
create **custom roles per-island** at runtime with `/is role create`: those are
stored on the island, not in this file. See
[Team & Roles](../commands/team-roles.md).

<Callout type="tip">

Run `/is admin reload` after editing to apply your changes.

</Callout>
