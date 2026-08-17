---
title: Account linking
order: 31
icon: link-2
---

Linking is the feature everything else depends on. A player runs `/link`, receives a six-digit code,
and enters it into a modal in Discord.

## The flow

1. `/link` in Minecraft generates a code valid for `code-expiration` seconds.
2. The player clicks **Link Account** in the linking channel.
3. A modal takes the code.
4. On success: the linked roles are assigned, `post-link-commands` run, a DM is sent if
   `send-dm-on-link` is on, and the audit webhook fires.

The direction matters. The code is issued in Minecraft, where the player has already proven they own
that account, and consumed in Discord. Someone who has only the Discord account cannot claim the
Minecraft one.

## Configuration

```yaml
discord:
  linking:
    channel-id: "..."
    linked-role-ids:
      - "..."
    code-expiration: 300
    send-dm-on-link: true
    notification-channel-id: ""
```

| Key | Meaning |
|---|---|
| `channel-id` | Where the embed with the button is posted |
| `linked-role-ids` | Roles given on a successful link — as many as you like |
| `code-expiration` | Seconds a code stays valid |
| `send-dm-on-link` | DM the user on success |
| `notification-channel-id` | Announce successful links here; empty disables it |

The embed, its button and the modal are all configurable — title, description, colour, thumbnail,
footer, the button's label, emoji and style (`PRIMARY`, `SECONDARY`, `SUCCESS`, `DANGER`), and the
modal's field labels.

`success-embed.thumbnail-url` defaults to `https://crafatar.com/avatars/%player_uuid%`, which renders
the player's skin. Any URL works.

## Post-link commands

```yaml
linking:
  post-link-commands:
    - "give %player% diamond 1"
    - "tellraw @a {\"text\":\"%player% linked their Discord account!\"}"
  allow-unlink: true
  post-unlink-commands:
    - "tellraw %player% {\"text\":\"Your account has been unlinked.\"}"
```

Run from console. Available placeholders: `%player%`, `%player_uuid%`, `%discord_id%`,
`%discord_user%`, `%server_name%`.

Rank-specific rewards go in `rank-sync.yml` instead — see [Rank sync](rank-sync.md#post-link-commands).

<Callout type="warning" title="Post-link commands and unlinking">

If `allow-unlink` is `true` and your post-link commands hand out items or money, a player can link,
claim, unlink and link again. Either make the reward idempotent — a permission, a role — or set
`allow-unlink: false` and handle unlinking through `/uxmdiscordsync forceunlink`.

</Callout>

## Rate limiting

```yaml
security:
  code-generation-rate-limit: 3
```

in `advanced.yml`. Three codes per minute per player. Expired codes are swept by the cleanup task on
`cleanup.cleanup-interval`.

## Checking and fixing links

```
/linkstatus                                        # the player checks their own
/uxmdiscordsync forcelink <player> <discord id>    # create a link directly
/uxmdiscordsync forceunlink <player>               # remove one
/uxmdiscordsync stats                              # how many links exist
```

`forcelink` is the answer when a player has lost access to the Discord account they linked with:
unlink the old pairing, then link the new one.
