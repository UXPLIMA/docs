---
title: Account linking
order: 31
description: How a code becomes a link, the link button and window, the rewards for linking and what unlinking undoes.
icon: link-2
---

Linking is what every other feature builds on. The player asks for a code in game and enters it in
Discord; from then on the plugin knows which Discord account belongs to which player.

## How a link is made

1. The player types `/link`. They get a code, valid for `code-expiration` seconds. Asking again
   replaces the old code.
2. In the link channel they press **Link Account**. A window opens with one field.
3. They type the code and send it.
4. The bot answers with the success message, visible only to them, and sends a copy as a direct
   message if `send-dm-on-link` is on.
5. The linked roles are added, the first-link rewards run, ranks and nickname are synced, and the
   audit log records the link.

| The bot answers | Why |
|---|---|
| The error message from `error-embed` | The code is wrong, expired or already used |
| That the Discord account is already linked, and to whom | One Discord account links to one Minecraft account. Unlink that one first |
| That the Minecraft account is already linked | The player behind the code has a link to another Discord account |
| To wait a number of seconds | Too many wrong codes, see below |

## Settings

In `config.yml`:

| Setting | Default | Meaning |
|---|---|---|
| `discord.linking.channel-id` | | The channel where the link button is posted |
| `discord.linking.linked-role-ids` | | Roles every linked member gets, and loses on unlinking |
| `discord.linking.code-expiration` | `300` | Seconds a code stays valid |
| `discord.linking.send-dm-on-link` | `true` | Also send the success message as a direct message |
| `linking.post-link-commands` | | Console commands for a player's first link |
| `linking.allow-unlink` | `true` | Whether players may use `/unlink`. Staff can always use `forceunlink` |
| `linking.post-unlink-commands` | | Console commands run when a link is removed |

In `advanced.yml`, `security.code-generation-rate-limit` is how many codes one player can ask for in
a minute, counted on every server together. `0` turns the limit off.

Codes are digits only and at least six long. After the third wrong code within ten minutes, a
Discord user has to wait 30 seconds, and again after every further wrong one, which keeps anyone from
guessing codes.

## The link button

The bot posts the link message by itself when it starts, and checks it again on every reload. If the
message in the channel no longer matches the config, the bot deletes it and posts a new one. Other
messages in the channel are never touched, so you can keep a rules message above it.

| Section | What you can change |
|---|---|
| `embed` | The link message itself. `%server_name%` shows the server name, or the network name with several servers |
| `button` | `label`, `emoji` (a plain emoji such as `"🔗"`, not a server emoji) and `style`: `PRIMARY`, `SECONDARY`, `SUCCESS` or `DANGER` |
| `modal` | The window's `title`, and the field's `label`, `placeholder`, `min-length` and `max-length` |
| `success-embed` | The answer after a link |
| `error-embed` | The answer to a wrong code |

The embeds take the keys listed in [Concepts](../getting-started/concepts.md#embeds).
`success-embed` takes `%player%`, `%player_uuid%`, `%discord%`, `%discord_id%`,
`%discord_mention%` and `%server_name%`, and PlaceholderAPI placeholders while the player is online.
The default thumbnail, `https://crafatar.com/avatars/%player_uuid%`, shows the player's head.

The code is always at least six digits, so `max-length` below 6 is raised to 6. Make the link channel
read-only for members: the button works without permission to write there.

## First-link rewards

```yaml
linking:
  post-link-commands:
    - "give %player% diamond 1"
```

These run as the console on the server the player is on, followed by the `post-link-commands` of
their highest-priority rank in `rank-sync.yml`. They run once per account, ever: an account linked
before gets nothing again, on either side. A player who is offline when the code is entered gets them
on their next join.

Placeholders: `%player%`, `%player_uuid%`, `%player_displayname%`, `%discord_id%`, `%discord_user%`,
`%server_name%`, `%online_players%`, `%max_players%`, and PlaceholderAPI placeholders.

## Unlinking

A link is removed with `/unlink` or `/dsadmin forceunlink <player>`. Unlinking:

| Undoes | Condition |
|---|---|
| The linked roles | Always |
| Every role from `rank-mappings` | Rank sync is on |
| The Discord nickname | `nickname-sync.reset-on-unlink` is on |
| Runs `post-unlink-commands` | Always, also for an offline player |

Use `post-unlink-commands` to take back what linking gave, such as a rank. The same placeholders
work, except PlaceholderAPI's while the player is offline.

## Fixing a link

| Situation | Command |
|---|---|
| A player wants to know their own link | `/linkstatus` |
| A player lost the Discord account they linked | `/dsadmin forcelink <player> <new discord id> <discord name>` |
| A link has to go | `/dsadmin forceunlink <player>` |
| You want the totals | `/dsadmin stats` |

Related: [Commands](../commands.md), [Rank and nickname sync](rank-sync.md)
