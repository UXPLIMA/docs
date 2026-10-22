---
title: Discord
order: 1550
description: The companion jar and what it mirrors into Discord.
icon: message-circle
---

uxmEssentials reaches Discord through a companion jar built on JDA. It mirrors server events into your
channels, so staff actions and money movement show up where your team already watches.

## What It Is

The bridge ships as its own jar, **`uxmessentials-discord`**, alongside the main
plugin. It is *not* bundled into the main jar and does nothing until configured.

It mirrors two kinds of event into Discord channels:

- **Audit events**: staff actions and sanctions, into the `audit` channel.
- **Economy notifications**: money movement, into the `economy` channel. A transaction
  below `min-eco-notify` is not posted.

`max-per-minute` caps how many messages the bridge posts, so a burst of events never
floods a channel.

Account linking is not part of uxmEssentials. It is sold as a plugin of its own.

## Setup (High Level)

1. Create a Discord **application + bot** in the Discord Developer Portal and copy
   its **bot token**.
2. Invite the bot to your server with permission to post in the channels you want.
3. Drop **`uxmessentials-discord`** into `plugins/` next to the main jar.
4. Put the token and the channel ids into the bridge's `discord.conf`, and set `enabled = true`.
5. Restart: JDA connects and notifications start to flow.

<Callout type="warning" title="Keep the bot token secret">

The token in `discord.conf` grants full control of the bot. Never commit it to
version control or paste it in a public channel. Rotate it in the Developer
Portal if it leaks.

</Callout>

## When the Bridge Is Absent

Because it's a distinct jar that stays dormant until configured, a server without it
is completely unaffected:

| Situation | Behavior |
|-----------|----------|
| Bridge not installed | Nothing is posted; every other feature works as before. |
| Bridge installed, no token | Dormant: no connection, no notifications. |
| Bridge connected | Audit and economy notifications flow into their channels. |

Related: [‍ Developer Overview](../developer/overview.md)
