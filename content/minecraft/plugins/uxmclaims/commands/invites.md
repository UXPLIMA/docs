---
title: Invites
order: 205
description: The invitation flow, from sending to accepting, and the inbox.
icon: mail
---

Nobody joins a claim directly. A member with the right to invite sends an invitation, and the invited
player accepts or rejects it. Accepting adds them with the claim's `Member` role.

## Commands

| Command | What it does | Claim access | Ability node |
|---|---|---|---|
| `/claim invite send <player>` | Invites a player to the current claim | `MANAGE_INVITES` | `uxmclaims.ability.member.invite` |
| `/claim invite revoke <player>` | Withdraws a pending invitation of the current claim | `MANAGE_INVITES` | `uxmclaims.ability.member.revoke` |
| `/claim invite accept <claim>` | Accepts an invitation, by claim name | none | none |
| `/claim invite reject <claim>` | Rejects an invitation, by claim name | none | none |
| `/claim invite inbox` | Opens your pending invitations | none | none |

`/claim trust <player>` is the same as `invite send`, and `/claim invites` the same as `invite inbox`.

### The flow

1. The owner, or a member whose role has `MANAGE_INVITES`, runs `/claim trust Steve`.
2. If Steve is online he gets `notificationInviteReceived`, with clickable **[Accept]** and
   **[Reject]** buttons. The buttons run `/claim invite accept <claim>` and
   `/claim invite reject <claim>`.
3. Steve accepts and becomes a member with the `Member` role.

If Steve was offline or missed the message, `/claim invites` lists everything waiting for him.
Invitations do not expire; they stay until accepted, rejected or revoked.

`send` refuses a player who is already a member, already invited, or banned from the claim.
`accept` checks the member limit again, so a claim that filled up meanwhile turns the invitation
down with `errorMaxMembers`.

## Permissions

| Node | Applies to | Default |
|---|---|---|
| `uxmclaims.limit.member.<n>` | Members in one claim, read from the owner | `50` |
| `uxmclaims.limit.invite.<n>` | Pending invitations in one claim, read from the owner | `10` |
| `uxmclaims.cost.invite.<count>.<price>` | Price of sending an invitation | `0.0` |
| `uxmclaims.delay.invite.<n>` | Seconds between invitations a player sends | `10` |

The invite limit counts invitations nobody has answered yet; revoking one frees its slot. See
[Limits, costs and delays](../permissions/limits.md).

## Notes

- The claim name in `accept` and `reject` is the last argument, so a name with spaces works:
  `/claim invite accept Claim of Alex`.
- `aliases.yml` ships `/accept <claim>` and `/deny <claim>`, which run `accept` and `reject`.
- Rewriting `notificationInviteReceived` in `messages.yml` changes what the invited player sees.
  Keep the two click commands if you want the buttons.
- The [invitation menus](../menus/invitations.md) send, revoke, accept and reject with the same checks.

Related: [Members](members.md), [Invitations menu](../menus/invitations.md), [aliases.yml](../config/aliases-yml.md)
