---
title: Bans
order: 408
description: The ban list, and what a ban actually blocks.
icon: user-x
---

Two screens: `menu/claim_ban_player.yml` to ban someone, `menu/claim_ban_list.yml` to see and lift
existing bans. Both need `MANAGE_BANS`.

| Action | Ability node |
|---|---|
| Ban | `uxmclaims.ability.member.ban` |
| Unban | `uxmclaims.ability.member.unban` |

## What a ban does

A ban is the strongest thing in the permission model. `Claim.hasPermission` checks it **first**, before
ownership and before roles: a banned player is denied everything inside the claim, no matter what
role they hold or what a per-member override says.

Banning a current member removes them from the claim as well.

## From chat

```
/claim ban Steve
/claim unban Steve
```

`/claim ban` is the one destructive action that does **not** ask for confirmation, deliberately: it
exists for the moment someone is already causing damage.

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
