---
title: webhooks.yml
order: 280
---

## How It Works

1. Something happens (claim created, member added, etc.)
2. Plugin checks if that event has a webhook configured
3. If yes, it sends a formatted message to Discord

---

## Setting Up Discord Webhooks

### Step 1: Create a Webhook in Discord

1. Go to your Discord server
2. Right-click the channel you want messages in
3. Click **Edit Channel** → **Integrations** → **Webhooks**
4. Click **New Webhook**
5. Give it a name (e.g., "Claims Bot")
6. Copy the **Webhook URL**

### Step 2: Configure in webhooks.yml

```yaml
senders:
  playerWebhook:
    username: 'uxmClaims'
    avatarUrl: 'https://your-image-url.com/avatar.png'
    webhookUrl: 'https://discord.com/api/webhooks/YOUR_WEBHOOK_URL'
```

| Setting      | What It Is                      |
|--------------|---------------------------------|
| `username`   | Name shown in Discord           |
| `avatarUrl`  | Profile picture URL             |
| `webhookUrl` | The URL you copied from Discord |

---

## Multiple Webhooks

You can have different webhooks for different channels:

```yaml
senders:
  playerWebhook:
    username: 'Claims'
    avatarUrl: 'https://example.com/player.png'
    webhookUrl: 'https://discord.com/api/webhooks/xxx'
    
  adminWebhook:
    username: 'Claims Admin'
    avatarUrl: 'https://example.com/admin.png'
    webhookUrl: 'https://discord.com/api/webhooks/yyy'
```

---

## Event Embeds

Each event type has its own embed configuration:

```yaml
embeds:
  ClaimCreateEvent:
    senders: [ 'playerWebhook' ]
    title: 'Claim Created'
    thumbnail: 'https://example.com/image.png'
    color: '#85e048'
    description:
      - 'Claim %claim.name% has been created'
      - 'ID: %claim.id%'
      - 'Owner: %claim.owner.name%'
    footer:
      text: 'Claim Created'
      icon: 'https://example.com/icon.png'
    author:
      name: 'uxmClaims'
      avatarUrl: 'https://example.com/avatar.png'
```

### Embed Settings

| Setting            | What It Is                   |
|--------------------|------------------------------|
| `senders`          | Which webhooks to use (list) |
| `title`            | Big title at top             |
| `thumbnail`        | Small image on right         |
| `color`            | Sidebar color (hex)          |
| `description`      | Main text (list of lines)    |
| `footer.text`      | Small text at bottom         |
| `footer.icon`      | Small icon next to footer    |
| `author.name`      | Author name at top           |
| `author.avatarUrl` | Author icon                  |

---

## All Available Events

### Claim Lifecycle

| Event                  | When It Fires     |
|------------------------|-------------------|
| `ClaimCreateEvent`     | New claim created |
| `ClaimDeleteEvent`     | Claim deleted     |
| `ClaimTimeExpireEvent` | Claim expired     |
| `ClaimNameChangeEvent` | Claim renamed     |

### Claim Blocks

| Event                        | When It Fires         |
|------------------------------|-----------------------|
| `ClaimBlockPlaceEvent`       | Claim block placed    |
| `ClaimBlockBreakEvent`       | Claim block destroyed |
| `ClaimExpireDateChangeEvent` | Expiration extended   |

### Members

| Event                    | When It Fires      |
|--------------------------|--------------------|
| `ClaimMemberAddEvent`    | Member joined      |
| `ClaimMemberRemoveEvent` | Member kicked/left |

### Chunks

| Event                   | When It Fires   |
|-------------------------|-----------------|
| `ClaimChunkCreateEvent` | Chunk claimed   |
| `ClaimChunkDeleteEvent` | Chunk unclaimed |

---

## Available Placeholders

Use these in your embed descriptions:

### Claim Placeholders

- `%claim.name%` - Claim name
- `%claim.id%` - Claim UUID
- `%claim.owner.name%` - Owner's name

### Member Placeholders

- `%claimMember.uid%` - Member's UUID
- `%claimMember.name%` - Member's name

### Chunk Placeholders

- `%chunkData.world.name%` - World name
- `%chunkData.x%` - Chunk X coordinate
- `%chunkData.z%` - Chunk Z coordinate

### Other

- `%old_name%` / `%new_name%` - For rename events
- `%old_expire_date%` / `%new_expire_date%` - For time change events

---

## Example: Complete Setup

```yaml
senders:
  claimChannel:
    username: 'Land Claims'
    avatarUrl: 'https://i.imgur.com/yourlogo.png'
    webhookUrl: 'https://discord.com/api/webhooks/xxx/yyy'

embeds:
  ClaimCreateEvent:
    senders: [ 'claimChannel' ]
    title: '🏠 New Claim!'
    color: '#00FF00'
    description:
      - '**%claim.owner.name%** claimed new land!'
      - ''
      - '📍 Name: %claim.name%'
    footer:
      text: 'Claim System'
      
  ClaimDeleteEvent:
    senders: [ 'claimChannel' ]
    title: '🗑️ Claim Deleted'
    color: '#FF0000'
    description:
      - 'Claim **%claim.name%** has been removed'
      - 'Owner: %claim.owner.name%'
```

---

## Tips

1. **Test your webhooks** - Make sure the URL works
2. **Use colors** - Match green for positive, red for negative events
3. **Keep it simple** - Don't spam every event
4. **Use thumbnails** - They make embeds look professional

---

## Disabling Webhooks

To disable a specific event, simply remove its section from `embeds:`.

To disable all webhooks, remove all entries from `senders:`.

---

## Next Steps

- [🎨 Menu Customization](../config/menus.md) - Customize menu appearance
- [🛡️ Protection Overview](../protection/overview.md) - How claim protection works
