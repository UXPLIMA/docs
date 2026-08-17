---
title: Members
order: 204
description: Roles, promotion, per-member overrides and ownership transfer.
icon: users
---

`/claim member` is the long form of the member shortcuts on the root command, plus the things the
shortcuts do not cover: promotion, role assignment and per-member permission overrides.

| Command | What it does | Ability node |
|---|---|---|
| `/claim member leave` | Leave the claim | n/a |
| `/claim member kick <player>` | Remove a member | `uxmclaims.ability.member.kick` |
| `/claim member promote <player>` | Move them one role up | `uxmclaims.ability.member.role` |
| `/claim member demote <player>` | Move them one role down | `uxmclaims.ability.member.role` |
| `/claim member setrole <player> <role>` | Put them in a specific role | `uxmclaims.ability.member.role` |
| `/claim member setperm <player> <perm> <true\|false>` | Override one permission for them | `uxmclaims.ability.member.permissions.<PERM>` |
| `/claim member transfer <player>` | Hand over ownership | `uxmclaims.ability.claim.transfer` |

## Promote and demote

Roles are ordered by **priority**, and lower is higher rank. `promote` moves a member to the role with
the next lower priority number; `demote` moves them to the next higher one. Neither can push someone
into `Owner`, and neither wraps around at the ends.

Because priority drives this, the order you see in `/claim role list` is the order promotion walks.
Set priorities deliberately: see [Roles](roles.md).

## Per-member overrides

`setperm` writes on the member, not the role, and the member's own list beats their role:

```
/claim member setrole Steve Builder          Steve gets everything Builder grants
/claim member setperm Steve CONTAINER_OPEN false   …except this
/claim member setperm Steve MANAGE_WARPS true      …plus this
```

Each member carries an **allowed** set and a **denied** set. Denied is checked first, so a denial
always wins, over the allowed set and over the role.

The ability node is per permission: `uxmclaims.ability.member.permissions.CONTAINER_OPEN` grants the
right to override exactly that one, and `…permissions.*` grants all of them.

## Ownership transfer

`transfer` confirms first, and refuses if:

- you are not the owner
- the target is you
- the target is not already a member of the claim

After the transfer the old owner becomes a normal member, not automatically an admin of their former
claim. Give them a role first if that is what you meant.

<Callout type="warning" title="A deleted role drops its members onto Member, not Default">

When a role is deleted, members holding it fall back to the built-in `Member` role, which by default
can build, break, and open every container in the claim. If you delete a restrictive custom role
expecting its holders to lose access, they gain it instead. Move them off the role first, then delete
it.

</Callout>
