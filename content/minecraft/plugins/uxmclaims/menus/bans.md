---
title: Bans
order: 408
description: The ban list, and what a ban actually blocks.
icon: user-x
---

`menu/claim_ban_list.yml`, opened by **Banned players** in claim management, lists every ban with its
reason and date. Clicking an entry lifts the ban after confirmation.

**Ban player** (`banPlayer`) asks for a name through `menu/claim_ban_player.yml`, which uses a dialog
and falls back to chat (see [typing text](overview.md#typing-text)). A ban typed there takes effect at
once; a ban from [member management](members-roles.md#managing-one-member) asks for confirmation first.

| Action | Role permission | Ability node |
|---|---|---|
| Ban | `MANAGE_BANS` | `uxmclaims.ability.member.ban` |
| Unban | `MANAGE_BANS` | `uxmclaims.ability.member.unban` |

## What a ban does

A ban is the strongest thing in the permission model. `Claim.hasPermission` checks it **first**, before
ownership and before roles: a banned player is denied everything inside the claim, no matter what
role they hold or what a per-member override says. A banned player cannot teleport into the claim by
any means.

Banning a player also removes their membership, cancels any invitation they hold, and moves them out of
the claim. The owner cannot be banned.

## From chat

```
/claim ban Steve
/claim unban Steve
```

`/claim ban` does **not** ask for confirmation, deliberately: it exists for the moment someone is
already causing damage.

## Limits

| | Node | Default |
|---|---|---|
| Bans per claim | `uxmclaims.limit.ban.<n>` | `25`, MAX |

The strategy is `MAX`, so ranks do not stack here: the best rank's number wins outright.

<Callout type="tip" title="A ban is not a substitute for the Default role">

A ban stops one named player. `Default` stops everyone you have not invited. If your answer to
griefing is a growing ban list, the `Default` role is granting too much: see
[roles.yml](../config/roles-yml.md).

</Callout>
