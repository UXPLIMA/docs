---
title: webhooks.yml
order: 505
description: Posting claim events to Discord as embeds.
icon: webhook
---

Off until you paste a webhook URL. A sender with an empty or malformed `webhookUrl` is skipped, so
nothing is sent out of the box.

## Senders

A sender is a Discord webhook plus the identity messages are posted under.

```yaml
senders:
  playerWebhook:
    username: 'uxmClaims'
    avatarUrl: ''
    webhookUrl: ''
  adminWebhook:
    username: 'uxmClaims Admin'
    avatarUrl: ''
    webhookUrl: ''
```

| Key | What it is |
|---|---|
| `username` | The name the post appears under; empty uses the webhook's own name |
| `avatarUrl` | The avatar image; empty or not an http(s) URL uses the webhook's own avatar |
| `webhookUrl` | The Discord webhook URL; required |

Two ship. Add as many as you like: the point of more than one is routing, player-visible events to a
public channel and deletions and expiries to a staff channel. Create the URL in Discord under
**Edit Channel → Integrations → Webhooks → New Webhook**.

## Embeds

Each block under `embeds` names the senders to post through and the embed to build.

| Key | What it is |
|---|---|
| `senders` | Which senders from above post this event |
| `title` | The embed title |
| `description` | A list of lines for the body |
| `color` | Hex sidebar colour, e.g. `'#85e048'` |
| `thumbnail` | Small image URL on the right |
| `image` | Large image URL at the bottom |
| `footer.text`, `footer.icon` | Footer; the icon shows only with text |
| `author.name`, `author.avatarUrl` | Author line above the title |

Image fields that are empty or not http(s) URLs are left out of the post rather than failing it.
Every embed carries the time it was sent.

Two placeholder roots work in the text fields: `%claim.*%` for the claim (`%claim.name%`,
`%claim.id%`, `%claim.owner.name%`, `%claim.spawnLocation.world%`) and `%event.*%` for the event's own
fields. The `date:` and `time:` modifiers use the formats from [messages.yml](messages-yml.md). A path
that resolves to nothing renders as the text `null`.

## The shipped events

The block key is the event's class name. Twelve ship:

| Key | Fires when | Event fields used |
|---|---|---|
| `ClaimCreateEvent` | A claim is created | |
| `ClaimDeleteEvent` | A claim is deleted | |
| `ClaimExpireEvent` | A claim expires | |
| `ClaimRenameEvent` | A claim is renamed | `%event.oldName%`, `%event.newName%` |
| `ClaimRescheduleEvent` | A claim's expiry changes | `%date:event.expireDate%` |
| `ClaimBlockPlaceEvent` | The claim block is placed | `%event.claimBlock.location.*%` |
| `ClaimBlockDestroyEvent` | The claim block is removed | `%event.claimBlock.location.*%` |
| `ClaimInviteAcceptEvent` | A player accepts an invitation and joins | `%event.member.name%`, `%event.member.uid%` |
| `ClaimMemberLeaveEvent` | A member leaves | `%event.member.name%`, `%event.member.uid%` |
| `ClaimMemberKickEvent` | A member is kicked | `%event.member.name%`, `%event.member.uid%` |
| `ClaimChunkExtendEvent` | A chunk is added | `%event.chunk.chunk.world%`, `.x`, `.z` |
| `ClaimChunkShrinkEvent` | A chunk is removed | `%event.claimChunk.chunk.world%`, `.x`, `.z` |

Any other claim event can be added the same way under its class name, for example
`ClaimOwnerTransferEvent` or `ClaimWarpCreateEvent`; the list is in [Events](../developer/events.md).
`ClaimMoveInsideEvent` and `ClaimMoveOutsideEvent` never post. A key that matches no event is
ignored.

**Deleting or commenting out a block disables that event.** There is no `enabled: false`.

## Notes

- **Start with two or three events.** `ClaimChunkExtendEvent` fires every time anyone buys a chunk,
  which on a busy server is a message every few seconds and a rate-limited webhook.
- **Route deletions and expiries to a staff channel.** They are the ones worth an audit trail: "my
  base is gone" is answerable in seconds if the deletion is in Discord with a timestamp.
- **Always write `senders`.** A block without it falls back to sender names that do not ship, and
  posts nothing.
- **Updating from an older release renames the shipped blocks.** The update replaces the old keys
  (`ClaimTimeExpireEvent`, `ClaimNameChangeEvent`, `ClaimMemberAddEvent` and the rest) with the ones
  above; check your routing afterwards.
- **Webhooks are a side effect, and side effects can be switched off in the API.** A plugin calling
  the facade with `ClaimCommandOptions` that exclude `WEBHOOK` performs the operation without posting.
  See [The API](../developer/api.md).

<Callout type="warning" title="A webhook URL is a credential">

Anyone holding it can post to that channel as your bot. Do not commit `webhooks.yml` to a public
repository, and regenerate the URL in Discord if it leaks.

</Callout>
