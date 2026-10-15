---
title: Members
order: 204
description: Removing members, moving them between roles, per-member overrides and ownership transfer.
icon: users
---

`/claim member` manages the people already in the current claim. Players join through an
[invite](invites.md); the short forms `/claim kick`, `/claim leave` and `/claim transfer` are on
[Claim commands](claim.md).

## Commands

| Command | What it does | Claim access | Ability node |
|---|---|---|---|
| `/claim member kick <player>` | Removes a member, after confirmation | `MANAGE_BANS` | `uxmclaims.ability.member.kick` |
| `/claim member promote <player>` | Moves a member one role up | owner | `uxmclaims.ability.member.role` |
| `/claim member demote <player>` | Moves a member one role down | owner | `uxmclaims.ability.member.role` |
| `/claim member setrole <player> <role>` | Puts a member in a named role | owner | `uxmclaims.ability.member.role` |
| `/claim member setperm <player> <perm> <true\|false>` | Overrides one role permission for one member | owner | `uxmclaims.ability.member.permissions.<perm>` |
| `/claim member transfer <player>` | Hands ownership to a member, after confirmation | owner | `uxmclaims.ability.claim.transfer` |
| `/claim member leave` | Leaves the claim | member | none |

`<player>` must be a member of the claim, and `<role>` a role name in it (case does not matter). The
owner's own role cannot be changed.

### Promote, demote and setrole

Roles are ordered by priority, and a lower number is a higher rank. `promote` moves a member to the
next role above their current one, and `demote` to the next role below. Neither moves anyone into
`Owner` or `Default`, and neither wraps around: at the ends you get `errorAlreadyHighestRole` or
`errorAlreadyLowestRole`. `setrole` follows the same rule and refuses `Owner` and `Default`.

The order is the one `/claim role setpriority` sets: see [Roles](roles.md#priority).

### Per-member overrides

`setperm` changes one permission for one member and leaves their role alone:

```
/claim member setrole Steve Builder
/claim member setperm Steve CONTAINER_OPEN false
/claim member setperm Steve MANAGE_WARPS true
```

Steve now has what `Builder` grants, minus `CONTAINER_OPEN`, plus `MANAGE_WARPS`.

Each member carries an allowed set and a denied set. A permission is checked against the denied set
first, then the allowed set, then the role, so a denial always wins. `setperm` only stores what
differs from the role: setting a permission to what the role already gives clears the override.

`<perm>` is one of the 48 [role permissions](../protection/permissions.md). The ability node uses the
lower-case name, for example `uxmclaims.ability.member.permissions.container_open`, and
`uxmclaims.ability.member.permissions.*` covers all of them.

### Ownership transfer

`transfer` refuses when you are not the owner (an admin who is not the owner is refused too), when the
target is you, and when the target is not a member. The new owner's claim limit applies, and you wait
`uxmclaims.delay.transfer` seconds (`300` by default) between transfers.

<Callout type="warning" title="A transfer cannot be taken back">

After a transfer the old owner holds the `Member` role and loses every owner-only action: flags,
roles, deleting the claim and transferring it back. Only the new owner can return it.

</Callout>

## Permissions

| Node | Applies to | Default |
|---|---|---|
| `uxmclaims.limit.claim.<n>` | Claims the transfer target may own | `1` |
| `uxmclaims.delay.transfer.<n>` | Seconds between transfers | `300` |

## Notes

- `/claim member leave` and `/claim leave` are the same action: no role permission or ability node,
  your selection is cleared, and `ClaimMemberLeaveEvent` fires. The owner cannot leave.
- A member whose role is deleted falls back to `Member`: see [Roles](roles.md).
- `/claim member kick` and `/claim kick` are the same action.
- The [member menu](../menus/members-roles.md) assigns roles and overrides with the same checks.

Related: [Roles](roles.md), [Invites](invites.md), [Role permissions](../protection/permissions.md)
