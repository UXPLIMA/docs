---
title: Roles
order: 206
description: Creating roles, ordering them by priority, and granting permissions.
icon: shield-user
---

Every claim has three system roles, `Owner`, `Member` and `Default`, and the owner can add custom
ones. `/claim role` creates, orders and configures them in the current claim; only the owner (or an
admin) can run these commands.

## Commands

| Command | What it does | Claim access | Ability node |
|---|---|---|---|
| `/claim role create <name>` | Creates a custom role with no permissions | owner | `uxmclaims.ability.role.create` |
| `/claim role delete <role>` | Deletes a custom role, after confirmation | owner | `uxmclaims.ability.role.delete` |
| `/claim role rename <role> <name>` | Renames a role | owner | `uxmclaims.ability.role.rename` |
| `/claim role setpriority <role> <n>` | Moves a role in the order | owner | `uxmclaims.ability.role.priority` |
| `/claim role setperm <role> <perm> <true\|false>` | Grants or removes one permission | owner | `uxmclaims.ability.role.permissions.<perm>` |

`<role>` is a role name in the claim, one word, any case. Role names are unique within a claim
regardless of case; a clash answers `errorRoleAlreadyExists`. System roles can be renamed but not
deleted.

### Priority

Priority orders the roles, and a lower number is a higher rank. A claim without custom roles has:

| Priority | Role |
|---|---|
| 0 | `Owner` |
| 1 | `Member` |
| 2 | `Default` |

A new role takes the place directly above `Default`, and `Default` moves down one. `setpriority`
accepts any number between `Owner` and `Default`, exclusive, and swaps the role with the one at that
number. `Owner` and `Default` cannot be moved; both cases answer `errorRolePriorityInvalid`. Deleting
a role moves every role below it up one.

Priority is the order `/claim member promote` and `demote` walk: see [Members](members.md).

### Granting permissions

```
/claim role create Builder
/claim role setperm Builder BLOCK_PLACE true
/claim role setperm Builder BLOCK_BREAK true
/claim role setperm Builder CONTAINER_OPEN true
```

`<perm>` is one of the 48 [role permissions](../protection/permissions.md), any case. The ability
node uses the lower-case name: `uxmclaims.ability.role.permissions.block_break` lets the owner grant
or remove exactly that one, and `uxmclaims.ability.role.permissions.*` covers all of them. This is how
a server keeps permissions such as `FLY` out of players' hands.

### The system roles

| Role | Who holds it | Notes |
|---|---|---|
| `Owner` | The claim owner | Has every permission; its own list is ignored |
| `Member` | Everyone who accepts an invite | Fallback for members of a deleted role, and the role a former owner gets after a transfer |
| `Default` | Every player who is not a member | Decides what strangers may do; ships with `MOVE_INSIDE` only |

The permissions each system role starts with come from [roles.yml](../config/roles-yml.md).

<Callout type="warning" title="A deleted role drops its members onto Member">

Members of a deleted role fall back to `Member`, which in the shipped `roles.yml` can build, break,
and open every container in the claim. Deleting a restrictive custom role gives its holders more
access, not less. Move them to another role first.

</Callout>

## Permissions

| Node | Applies to | Default |
|---|---|---|
| `uxmclaims.limit.role.<n>` | Custom roles per claim, read from the owner | `5` |
| `uxmclaims.cost.role.<count>.<price>` | Price of creating a role | `0.0` |
| `uxmclaims.delay.rolerename.<n>` | Seconds between role renames | `30` |

The role limit counts custom roles only. See [Limits, costs and delays](../permissions/limits.md).

## Notes

- `Default` is the only role a griefer ever has. Keep it at `MOVE_INSIDE`, or remove that too to keep
  strangers out of the claim entirely.
- `/claim admin bulk setrolepermission` changes one permission on a system role in every claim: see
  [Admin](admin.md).
- The [role menus](../menus/members-roles.md) create, order and edit roles with the same checks.

Related: [Members](members.md), [Role permissions](../protection/permissions.md), [roles.yml](../config/roles-yml.md)
