---
title: Discord
order: 1550
description: 'uxmEssentials can reach into Discord through a small companion jar built
  on JDA (the Java Discord API). It mirrors server events into your Discord channels and
  hosts the Discord side of account linking. It''s entirely optional: a separate
  download that sits dormant until you give it a bot token.'
---

---

## What It Is

The bridge ships as its own jar, **`uxmessentials-discord`**, alongside the main
plugin. It is *not* bundled into the main jar and does nothing until configured.

It does two things:

- **Notifications**: mirrors audit events and economy notifications into Discord
  channels, so staff actions and money movement show up where your team already
  watches.
- **Account linking**: hosts the Discord-side `/link` slash command that redeems
  the code a player generates in-game.

---

## How Linking Works

The in-game and Discord halves are deliberately decoupled:

1. A player runs `/discordlink` in-game to get a one-time code.
2. They run `/link <code>` in your Discord server.
3. The bridge redeems the code and binds the two accounts.

The in-game commands (`/discordlink`, `/discordlink status`, `/discordlink gui`,
`/discordunlink`) come from the **Discord Link** module and work on their own: a
player can generate and inspect a code with no bridge running. Only the **redemption
step** needs the companion jar to be online and connected.

| Command | Where | Permission |
|---------|-------|------------|
| `/discordlink` · `/discordlink status` | In-game | `uxmessentials.discord.link` |
| `/discordlink gui` | In-game | `uxmessentials.discord.gui` |
| `/discordunlink` | In-game | `uxmessentials.discord.link` |
| `/link <code>` | Discord | (Discord role gating) |

---

## Setup (High Level)

1. Create a Discord **application + bot** in the Discord Developer Portal and copy
   its **bot token**.
2. Invite the bot to your server with permission to post in the channels you want.
3. Drop **`uxmessentials-discord`** into `plugins/` next to the main jar.
4. Put the token and channel settings into the bridge's `discord.conf`.
5. Restart: JDA connects and the `/link` slash command registers.

<Callout type="warning" title="Keep the bot token secret">

The token in `discord.conf` grants full control of the bot. Never commit it to
version control or paste it in a public channel. Rotate it in the Developer
Portal if it leaks.

</Callout>

---

## When the Bridge Is Absent

Because it's a distinct jar that stays dormant until configured, a server without it
is completely unaffected:

| Situation | Behavior |
|-----------|----------|
| Bridge not installed | In-game `/discordlink` still issues codes; there is simply nothing to redeem them against. |
| Bridge installed, no token | Dormant: no connection, no notifications. |
| Bridge connected | Notifications flow and `/link` redeems codes. |

---

## Setup Checklist

1. Keep the **Discord Link** module enabled for the in-game commands.
2. Install the **`uxmessentials-discord`** jar only if you want notifications and
   `/link` redemption.
3. Configure a bot token in `discord.conf`.
4. Restart and confirm the bot comes online in your Discord server.

---

## Next Steps

- [🔗 Discord Link Feature](../features/discord-link.md): the in-game linking flow in full
- [👨‍💻 Developer Overview](../developer/overview.md): the linking service seam shared over `ServicesManager`
