---
title: Discord Link
order: 9040
description: Let a player bind their Minecraft account to their Discord account with a one-time code.
---

Discord Link lets a player bind their own accounts without a staff ticket: they run `/discordlink`, copy the
one-time code, and hand it to the Discord bot's own link command. It is the foundation for Discord-side perks
like role sync and verified-member gates.

Module `discordlink` · disabled by default · `modules/discordlink/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/discordlink` | Generate a code to link your account to Discord. | `uxmessentials.discord.link` |
| `/discordunlink` | Remove the link between your account and Discord. | `uxmessentials.discord.link` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.discord.gui` | everyone | /discordlink gui (and the discordlink entry on the /uxmess gui hub) to open the link-status panel: your binding, a generate-code button, and a confirm-gated unlink. |
| `uxmessentials.discord.link` | everyone | /discordlink (issue a code), /discordlink status (show your binding), and /discordunlink (remove it): all act only on your own account. |
| `uxmessentials.module.discordlink` | op | Hot-reload / inspect the discordlink module (account linking and Discord notifications). |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `code-ttl-seconds` | `600` | How long, in seconds, a generated link code stays valid before it expires and the player must run /discordlink again for a fresh one. A shorter window narrows the chance of a leaked code being redeemed; a longer one is more forgiving if the player takes a moment to switch to Discord. Default 600 = 10 minutes; minimum 1. |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_discordlink_id%` | The bound Discord user id. |
| `%uxmessentials_discordlink_linked%` | Whether the account is bound to a Discord user (yes/no). |
{/* /generated */}

## Notes

- **Two pieces make the feature.** The commands live in the main plugin, the redemption happens in the
  `uxmessentials-discord` bridge jar. Codes are issued even before the bridge is installed; they just cannot be
  redeemed until it is.
- **The bridge needs a bot token in `discord.conf`** and stays dormant without one.
- **Redemption answers with a precise outcome:** linked, no such code, expired, already linked or malformed, so
  a player is told what went wrong rather than that it failed.
- **A binding survives restarts,** because it is a database row rather than session state.
- **The same bridge mirrors audit and economy notices into Discord channels,** so linking is one part of a
  broader integration rather than a standalone add-on.

Related: [Security](security.md), [Vote](vote.md), [Communication](communication.md)
