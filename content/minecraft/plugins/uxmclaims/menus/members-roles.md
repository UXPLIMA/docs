---
title: Members and roles
order: 406
description: The member list, per-member overrides, and the role editor.
icon: users
---

## The member list

`menu/claim_member_list.yml`. Paginated, one head per member, showing their role and join date.

Click a member to manage them.

## Managing one member

`menu/claim_member_manage.yml`. Two things live here:

- **Their role.** Assign any role in the claim, or promote and demote through the priority order.
- **Their permission overrides.** Every one of the 48
  [role permissions](../protection/permissions.md) is listed with its effective state, and clicking
  writes an override on the member.

An override beats the role. The member carries an *allowed* set and a *denied* set, and denied is
checked first, so a denial holds even if you later promote them.

The state shown is the effective one: the override if there is one, otherwise the role's value,
otherwise the `Member` role's value if their role no longer exists.

| Action | Ability node |
|---|---|
| Change role | `uxmclaims.ability.member.role` |
| Override one permission | `uxmclaims.ability.member.permissions.<PERM>` |
| Remove the member | `uxmclaims.ability.member.kick` |

## The role list

`menu/claim_role_list.yml`. Every role in the claim, ordered by priority: lower first, so `Owner`,
`Member`, `Default` and anything custom in between.

| Button | Does |
|---|---|
| A role | Opens its editor |
| Create | Prompts in chat for a name |

## The role editor

`menu/claim_role_manage.yml`. All 48 permissions with their current state; click to toggle. Rename
and priority live here too, in `claim_role_rename.yml` and the priority control.

| Action | Ability node |
|---|---|
| Toggle a permission | `uxmclaims.ability.role.permissions.<PERM>` |
| Rename | `uxmclaims.ability.role.rename` |
| Change priority | `uxmclaims.ability.role.priority` |
| Delete | `uxmclaims.ability.role.delete` |

Per-permission ability nodes are what stop a member with role-editing rights from quietly granting
themselves `MANAGE_VAULT`.

## Limits

| | Node | Default |
|---|---|---|
| Members per claim | `uxmclaims.limit.member.<n>` | `50`, stacking |
| Custom roles per claim | `uxmclaims.limit.role.<n>` | `5`, MAX |

<Callout type="warning" title="Deleting a role promotes its members">

Members holding a deleted role fall back to the built-in `Member` role, which by default may build,
break and open every container. Move them somewhere else before deleting a restrictive role.

</Callout>
