---
title: webhooks.yml
order: 505
description: Posting claim events to Discord as embeds.
icon: webhook
---

Off until you paste a webhook URL. Nothing is sent otherwise.

## Senders

A sender is a Discord webhook plus the identity messages are posted under.

```yaml
senders:
  playerWebhook:
    username: 'uxmClaims'
    avatarUrl: 'AVATAR_URL'
    webhookUrl: 'WEBHOOK_URL'
  adminWebhook:
    username: 'uxmClaims Admin'
    avatarUrl: 'AVATAR_URL'
    webhookUrl: 'WEBHOOK_URL'
```

Two ship. Add as many as you like: the point of more than one is routing: player-visible events to a
public channel, deletions and expiries to a staff channel.

Create the URL in Discord under **Edit Channel → Integrations → Webhooks → New Webhook**.

## Embeds

Each event gets a block naming the senders to post through and the embed to build.

| Key | What it is |
|---|---|
| `senders` | Which senders from above receive this event |
| `title` | The embed title |
| `description` | A list of lines for the body |
| `color` | Hex sidebar colour, e.g. `'#85e048'` |
| `thumbnail` | Small image on the right |
| `image` | Large image at the bottom |
| `footer.text`, `footer.icon` | Footer |
| `author.name`, `author.avatarUrl` | Author line above the title |

Placeholders work in every text field, `%claim.name%`, `%claim.owner.name%`,
`%count:claim.chunks%` and the rest. See [Placeholders](../placeholders/reference.md).

## The events

| Key | Fires when |
|---|---|
| `ClaimCreateEvent` | A claim is created |
| `ClaimDeleteEvent` | A claim is deleted |
| `ClaimTimeExpireEvent` | A claim expires |
| `ClaimNameChangeEvent` | A claim is renamed |
| `ClaimExpireDateChangeEvent` | A claim's lifetime is extended |
| `ClaimBlockPlaceEvent` | The claim block is placed |
| `ClaimBlockBreakEvent` | The claim block is removed |
| `ClaimMemberAddEvent` | A member joins |
| `ClaimMemberRemoveEvent` | A member leaves or is kicked |
| `ClaimChunkCreateEvent` | A chunk is added |
| `ClaimChunkDeleteEvent` | A chunk is removed |

**Deleting or commenting out a block disables that event.** There is no `enabled: false`.

## Notes

- **Start with two or three events.** `ClaimChunkCreateEvent` fires every time anyone buys a chunk,
  which on a busy server is a message every few seconds and a rate-limited webhook.

- **Route deletions and expiries to a staff channel.** They are the ones worth an audit trail:
  "my base is gone" is answerable in seconds if the deletion is in Discord with a timestamp.

- **Webhooks are a side effect, and side effects can be switched off in the API.** A plugin calling
  the facade with `ClaimCommandOptions` that exclude `WEBHOOK` performs the operation without posting.
  See [The API](../developer/api.md).

<Callout type="warning" title="A webhook URL is a credential">

Anyone holding it can post to that channel as your bot. Do not commit `webhooks.yml` to a public
repository, and regenerate the URL in Discord if it leaks.

</Callout>
