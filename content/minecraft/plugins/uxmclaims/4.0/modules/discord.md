---
title: Discord
order: 1348
description: Town, nation and ally chat to Discord and back, event notices, and with the plugin's own bot a channel and role per group.
icon: message-circle
---

The discord module carries town, nation and ally chat between the game and Discord channels and posts notices
of town, nation, war and land events. It runs on DiscordSRV's bot, or on a bot of its own that also makes a
private channel and a role for every town and nation and links game accounts to Discord accounts.

Module `discord` · disabled by default · `modules/discord/config.yml`

Chat carrying needs [Group Chat](group-chat.md); town, nation and ally parts need [Towns](towns.md),
[Nations](nations.md) and [Relations](relations.md).

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/town discord link <channel>` | Links your town's chat to a Discord channel. Mayor only. | `uxmclaims.command.discord` |
| `/town discord unlink` | Unlinks your town's chat. | `uxmclaims.command.discord` |
| `/town discord ally link <channel>` | Links your town's ally chat to a channel. | `uxmclaims.command.discord` |
| `/town discord ally unlink` | Unlinks your town's ally chat. | `uxmclaims.command.discord` |
| `/nation discord link <channel>` | Links your nation's chat to a channel. Nation leader only. | `uxmclaims.command.discord` |
| `/nation discord unlink` | Unlinks your nation's chat. | `uxmclaims.command.discord` |
| `/nation discord ally link <channel>` | Links your nation's ally chat to a channel. | `uxmclaims.command.discord` |
| `/nation discord ally unlink` | Unlinks your nation's ally chat. | `uxmclaims.command.discord` |
| `/discord link` | Gives you a code to send to the bot in a direct message (`backend: bot`). | `uxmclaims.command.discord.link` |
| `/discord unlink` | Unlinks your Discord account. | `uxmclaims.command.discord.link` |
| `/uxm discord status` | Shows the backend, the connection and the links. | `uxmclaims.admin.discord` |
| `/uxm discord test` | Posts a test line to the notice channel. | `uxmclaims.admin.discord` |
| `/uxm discord sync` | Makes the missing channels and roles of every town and nation and gives linked players their roles. | `uxmclaims.admin.discord` |
| `/uxm discord link <town\|nation> <name> <channel>` | Links any town's or nation's chat. | `uxmclaims.admin.discord` |
| `/uxm discord unlink <town\|nation> <name>` | Unlinks it. | `uxmclaims.admin.discord` |
| `/uxm discord ally link <town\|nation> <name> <channel>` | Links any group's ally chat. | `uxmclaims.admin.discord` |
| `/uxm discord ally unlink <town\|nation> <name>` | Unlinks it. | `uxmclaims.admin.discord` |
| `/uxm discord account unlink <player>` | Unlinks another player's Discord account, by player name or Discord user id. | `uxmclaims.admin.discord` |

A `<channel>` is a DiscordSRV channel name (from the `Channels` setting in DiscordSRV's `config.yml`, such as
`global`), a channel name on the bot's Discord server, or a channel id. A channel holds one link: it cannot be
both a chat channel and an ally channel, or belong to two groups.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.command.discord` | everyone | `/town discord` and `/nation discord` |
| `uxmclaims.command.discord.link` | everyone | `/discord link` and `/discord unlink` |
| `uxmclaims.admin.discord` | op | `/uxm discord` |

## Settings

| Key | Default | What it does |
|---|---|---|
| `backend` | `discordsrv` | `discordsrv`: DiscordSRV's bot, channels and account links. `bot`: the plugin's own bot. Needs a restart. |
| `bot.token` | `''` | The bot token. Empty: read from the `UXM_DISCORD_TOKEN` environment variable. Never logged. |
| `bot.guild-id` | `''` | The id of the Discord server the bot serves. |
| `bot.message-content-intent` | `true` | Asks for the Message Content intent. Turn it on for the bot in the developer portal, or lines written in Discord never reach the game. |
| `bot.request-timeout` | `15s` | How long one request to Discord may take. |
| `chat.town.enabled` | `true` | Carries town chat. |
| `chat.town.shared-channel` | `''` | A channel every town without its own link posts to. Lines written there are not sent into the game. |
| `chat.town.links` | `{}` | Two-way links, town name or id to channel. Links made by command win over these. |
| `chat.nation.*` | | The same three keys for nation chat. |
| `chat.ally.*` | | The same three keys for ally chat. |
| `chat.to-discord` | `**[%group%] %player%:** %message%` | How a game line looks in Discord. `%channel%` is town, nation or ally. |
| `chat.from-discord-name` | `%name%` | The name a Discord line shows in the game: the Discord name, or the game name of a linked account. |
| `chat.require-linked-account` | `false` | Only Discord users with a linked account may talk into the game. |
| `chat.max-length` | `256` | Longer Discord lines are cut. |
| `notices.enabled` | `true` | Sends event notices. |
| `notices.channel` | `global` | The channel of a notice that names none. |
| `notices.group-channels` | `true` | Notices of a group's own events also go to its linked chat channel when the notice allows it. A war notice goes to both sides. |
| `guild.auto-channels` | `false` | Makes a private channel and a role for every town and nation when it is founded, renames them with it, and links the channel to its chat. |
| `guild.category-id` | `''` | The category the channels go under. Empty: the bot makes one named `category-name` once and remembers it. |
| `guild.category-name` | `uxmClaims` | Name of that category. |
| `guild.town-channel-name` | `town-%name%` | Town channel name. Discord lower-cases it. |
| `guild.nation-channel-name` | `nation-%name%` | Nation channel name. |
| `guild.channel-topic` | `uxmClaims: %name%` | Channel topic. |
| `guild.town-role-name` | `%name%` | Town role name. |
| `guild.nation-role-name` | `%name% (nation)` | Nation role name. |
| `guild.on-delete` | `delete` | When a town or nation is deleted: `delete` or `keep` its channel and role. |
| `guild.roles` | `true` | Gives players with a linked account their town's and nation's role, and takes it when they leave. |
| `guild.sync-batch` | `3` | How many groups `/uxm discord sync` works on at once. |
| `linking.code-lifetime` | `10m` | How long a `/discord link` code stays valid. |

## Notices

`notices.yml` holds one entry per event, keyed by the event's class name. Each has `enabled`, `channel` (empty
for `notices.channel`), `group-channel` and `text` (Discord markdown with placeholders such as `%town%`,
`%nation%`, `%player%`, `%actor%`, `%land.owner%`). Values are escaped, so names show as written and nobody gets
pinged. Any uxmClaims event that reports a finished change can be added, including those of other modules.

| On by default | Off by default |
|---|---|
| Town create, delete, rename, mayor change; nation create, delete, rename, join, leave; relation change; war declare, start, end | Town join, leave, kick; nation capital change; war point capture; land create, delete, owner transfer; ruin; bankruptcy |

## Choosing a backend

With `backend: discordsrv` you need DiscordSRV installed; channel names and account links are DiscordSRV's, and
`/discord link` points players there. Without DiscordSRV the module logs one line and stays idle.

With `backend: bot`, create an application in the Discord developer portal, add the bot to your server, turn on
the Message Content intent, and set `bot.token` (or `UXM_DISCORD_TOKEN`) and `bot.guild-id`. Invite the bot
with these permissions:

| Permission | Needed for |
|---|---|
| View Channels, Send Messages, Read Message History | Chat and notices |
| Manage Channels | `guild.auto-channels`: a channel per town and nation and its permission overwrites |
| Manage Roles | `guild.roles`: a role per town and nation, given to linked members. The bot's own role must be above those roles |

Players link with `/discord link` and send the code to the bot in a direct message.

## Notes

- `backend`, the token and the guild id are read at module start. `/uxm reload` does not switch the backend or
  reconnect the bot; restart the server. `/uxm discord status` says when `config.yml` names another backend or
  Discord server than the running one.
- On a network with `backend: bot`, every server starts its own bot and reacts to the same events. Run the module
  on one server only. Account links made on another server show after a restart.
- Only plain text messages reach the game. Edits, deletes, attachments, embeds and stickers are not carried.
- Roles changed by hand in Discord are not read back. A channel or category deleted by hand stays remembered:
  unlink the channel, or set `guild.category-id`, before new channels can be made.
- A channel the bot restricts to its town or nation loses any permission overwrites you added by hand.

Related: [Group Chat](group-chat.md), [Towns](towns.md), [Nations](nations.md), [Webhooks](webhooks.md),
[Integrations](../integrations.md)
