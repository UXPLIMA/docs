---
title: Invitations
order: 407
description: Sending invitations and cancelling ones that have not been answered.
icon: mail
---

Two screens, plus the inbox on the personal menu.

## Sending

`menu/claim_invite_send.yml`, opened from claim management. Pick a player and the invitation goes out.

Needs `MANAGE_INVITES` and `uxmclaims.ability.member.invite`.

The invited player, if online, gets `notificationInviteReceived` from `messages.yml` — a message with
clickable **[Accept]** and **[Reject]** buttons running `/claim invite accept` and
`/claim invite reject`.

## Pending

`menu/claim_invite_list.yml`. Every invitation this claim has sent that has not been answered, with
the target's head and the time it was sent.

Clicking revokes it, which frees a slot against the invite limit. Needs
`uxmclaims.ability.member.revoke`.

## The inbox

`menu/common_invite_inbox.yml`, opened by `/claim invites` from the personal menu. Everything waiting
for *you*, with accept and reject on each entry.

This is where a player who was offline when they were invited finds out.

## Limits

| | Node | Default |
|---|---|---|
| Pending invitations per claim | `uxmclaims.limit.invite.<n>` | `10`, MAX |
| Cost of sending one | `uxmclaims.cost.invite.<count>.<price>` | `0.0` |
| Members per claim | `uxmclaims.limit.member.<n>` | `50`, stacking |

The invite limit counts unanswered invitations, so a claim inviting offline players in bulk will hit
it. Revoking frees a slot.

<Callout type="info" title="Nobody joins without accepting">

`/claim trust` and this screen both create an invitation, not a membership. Until the player accepts
they hold no permissions at all and are subject to the `Default` role like any stranger.

</Callout>
