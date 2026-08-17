---
title: Invites
order: 205
description: The invitation flow, from sending to accepting, and the inbox.
icon: mail
---

Nobody is added to a claim directly. An invitation is created, and the invited player accepts it.

| Command | What it does | Ability node |
|---|---|---|
| `/claim invite send <player>` | Invite someone | `uxmclaims.ability.member.invite` |
| `/claim invite accept <claim>` | Accept an invitation | — |
| `/claim invite reject <claim>` | Decline one | — |
| `/claim invite revoke <player>` | Withdraw one you sent | `uxmclaims.ability.member.revoke` |
| `/claim invite inbox` | Open your pending invitations | — |

`/claim trust <player>` is a shorthand for `invite send`, and `/claim invites` for `invite inbox`.

Sending also needs the `MANAGE_INVITES` role permission.

## The flow

1. The owner or a member with `MANAGE_INVITES` runs `/claim trust Steve`.
2. Steve, if online, receives a message with clickable **[Accept]** and **[Reject]** buttons.
3. Steve accepts. He becomes a member with the claim's `Member` role.

The notification is `notificationInviteReceived` in `messages.yml` and its buttons run
`/claim invite accept <claim>` and `/claim invite reject <claim>`. Rewriting that message is how you
change what players see; the click targets are ordinary commands.

If Steve was offline, or lost the message, `/claim invites` shows everything waiting.

## Limits

| Limit | Node | Default |
|---|---|---|
| Pending invitations per claim | `uxmclaims.limit.invite.<n>` | `10`, MAX |
| Members per claim | `uxmclaims.limit.member.<n>` | `50`, stacking |
| Cost of sending one | `uxmclaims.cost.invite.<count>.<price>` | `0.0` |

The invite limit counts invitations that have not been answered, so a claim spamming invitations to
offline players will hit it. Revoking frees a slot.

<Callout type="info" title="Aliases make this two keystrokes">

`aliases.yml` ships `/accept` and `/untrust`. Adding `/invite: "claim invite send"` gives players the
verb they expect from every other plugin, without touching the command tree.

</Callout>
