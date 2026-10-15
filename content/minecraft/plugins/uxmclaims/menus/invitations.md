---
title: Invitations
order: 407
description: Sending invitations and cancelling ones that have not been answered.
icon: mail
---

A claim's pending list, the text input that sends an invitation, and each player's inbox.

## Pending

`menu/claim_invite_list.yml`, opened by **Invitations** in claim management. Every invitation this
claim has sent that has not been answered, with the target's head and the time it was sent.

Clicking an entry revokes it after confirmation, which frees a slot against the invite limit.

## Sending

**Invite player** (`invitePlayer`) in the pending list, or **Add member** in the member list, asks for a
player name through `menu/claim_invite_send.yml`. The shipped file uses a dialog and falls back to
chat; see [typing text](overview.md#typing-text).

From either menu, the invited player, if online, gets `notificationInviteReceived`
from `messages.yml`: a message with clickable **[Accept]** and **[Reject]** buttons running
`/claim invite accept` and `/claim invite reject`.

| Action | Role permission | Ability node |
|---|---|---|
| Send | `MANAGE_INVITES` | `uxmclaims.ability.member.invite` |
| Revoke | `MANAGE_INVITES` | `uxmclaims.ability.member.revoke` |

## The inbox

`menu/common_invite_inbox.yml`, opened from the [personal menu](personal.md), `/claim invites` or
`/claim invite inbox`. Everything waiting for *you*: left click accepts, right click declines.

This is where a player who was offline when they were invited finds out.

## Limits

| | Node | Default |
|---|---|---|
| Pending invitations per claim | `uxmclaims.limit.invite.<n>` | `10`, MAX |
| Cost of sending one | `uxmclaims.cost.invite.<n>.<price>` | `0.0` |
| Members per claim | `uxmclaims.limit.member.<n>` | `50`, stacking |

The invite limit counts unanswered invitations, so a claim inviting offline players in bulk will hit
it. Revoking frees a slot.

<Callout type="info" title="Nobody joins without accepting">

`/claim trust` and this screen both create an invitation, not a membership. Until the player accepts
they hold no permissions at all and are subject to the `Default` role like any stranger.

</Callout>
