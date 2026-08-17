---
title: Roles
order: 206
description: Creating roles, ordering them by priority, and granting permissions.
icon: shield-user
---

| Command | What it does | Ability node |
|---|---|---|
| `/claim role create <name>` | Create a role with no permissions | `uxmclaims.ability.role.create` |
| `/claim role delete <role>` | Delete a role | `uxmclaims.ability.role.delete` |
| `/claim role rename <role> <name>` | Rename it | `uxmclaims.ability.role.rename` |
| `/claim role setpriority <role> <n>` | Move it in the hierarchy | `uxmclaims.ability.role.priority` |
| `/claim role setperm <role> <perm> <true\|false>` | Grant or revoke one permission | `uxmclaims.ability.role.permissions.<PERM>` |

A new role starts empty. It grants nothing until you add permissions.

## Priority

Priority orders the hierarchy and **lower is higher rank**:

| Priority | Role |
|---|---|
| 0 | `Owner` |
| 1 | `Member` |
| 2 | `Default` |

Custom roles are placed between `Member` and `Default` when the claim is created from
[`roles.yml`](../config/roles-yml.md), and `setpriority` moves them afterwards.

Priority is what `/claim member promote` and `demote` walk, and it is also what decides who wins a
contested vault: opening the vault while a higher-priority member has it open answers *"Someone with a
higher rank opened the vault."*

## Granting permissions

```
/claim role create Builder
/claim role setperm Builder BLOCK_PLACE true
/claim role setperm Builder BLOCK_BREAK true
/claim role setperm Builder CONTAINER_OPEN true
```

Tab completion offers all 48 permissions. Names are the enum names: see
[Role permissions](../protection/permissions.md) for what each one covers.

The ability node is per permission: `uxmclaims.ability.role.permissions.BLOCK_BREAK` lets a player
grant exactly that one to a role, and `…permissions.*` lets them grant anything. This is what stops a
claim member with role-editing rights from quietly promoting themselves.

## The three roles you cannot delete

`Owner`, `Member` and `Default` are system roles. They can be renamed and their permissions changed,
but not removed:

- `Owner` holds every permission implicitly; its permission list is empty by design, and adding to it
  changes nothing.
- `Member` is the fallback for a member whose role was deleted.
- `Default` applies to everyone who is **not** a member: the strangers walking through.

## Limits

| Limit | Node | Default |
|---|---|---|
| Custom roles per claim | `uxmclaims.limit.role.<n>` | `5`, MAX |
| Cost of creating one | `uxmclaims.cost.role.<count>.<price>` | `0.0` |

The limit counts custom roles only; the three system roles do not use a slot.

<Callout type="tip" title="Design Default before you design anything else">

`Default` is the only role a griefer will ever have. Everything a claim needs to be safe is decided
there. The shipped default grants `MOVE_INSIDE` and nothing else, which is the right starting point:
strangers may walk through and do nothing at all.

</Callout>
