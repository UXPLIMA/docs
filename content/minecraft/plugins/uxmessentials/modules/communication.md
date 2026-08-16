---
title: Communication
order: 9020
description: The public chat format, join and quit lines, the rotating announcer, info pages and broadcasts.
icon: megaphone
---

Communication owns everything the server says to players that is not a private message: how public chat is
formatted, the join and quit lines, the announcer that posts on a timer, static pages like `/rules` and `/motd`,
chat control, and one-off broadcasts. It is driven by several HOCON files under `modules/communication/`, and
every line is MiniMessage.

Module `communication` · enabled by default · `modules/communication/config.conf`, `chat.conf`, `announcer.conf`, `info-pages.conf`, `advancements.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/announce` | Manage the rotating server announcer. | `uxmessentials.announce.admin` |
| `/broadcast` | Send an announcement to all online players. | `uxmessentials.communication.broadcast` |
| `/broadcasttoggle` | Toggle whether you receive server announcements. | `uxmessentials.communication.broadcasttoggle` |
| `/broadcastworld` (`/bcw`) | Send an announcement to players in your world. | `uxmessentials.communication.broadcastworld` |
| `/clearchat` (`/chatclear`) | Flush the chat for online players. | `uxmessentials.communication.clearchat` |
| `/communication` | Open the communication admin panel. | `uxmessentials.communication.gui` |
| `/me` | Broadcast an action message about yourself. | `uxmessentials.communication.me` |
| `/togglechat` (`/mutechat`) | Lock or unlock public chat for non-staff. | `uxmessentials.communication.togglechat` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.announce.admin` | op | /announce reload\|list\|preview\|toggle to manage the rotating announcer. |
| `uxmessentials.announce.admin.editor` | everyone | /announce editor to author announcements in the GUI. |
| `uxmessentials.announce.admin.list` | everyone | /announce list to read the rotation and the channels each announcement pushes to. |
| `uxmessentials.announce.admin.preview` | everyone | /announce preview \<id> to show one announcement to yourself alone. |
| `uxmessentials.announce.admin.reload` | everyone | /announce reload to re-read announcer.conf and re-arm the rotation. |
| `uxmessentials.announce.admin.toggle` | everyone | /announce toggle to stop or resume receiving the rotating announcements. |
| `uxmessentials.communication.broadcast` | op | /broadcast to send a one-off announcement to all online players. |
| `uxmessentials.communication.broadcasttoggle` | everyone | /broadcasttoggle to stop or resume receiving the rotating server announcements. |
| `uxmessentials.communication.broadcastworld` | op | /broadcastworld (alias /bcw) to send a one-off announcement only to players in your world. |
| `uxmessentials.communication.chat.bypass` | op | Keep chatting while public chat is locked by /togglechat. |
| `uxmessentials.communication.chat.format` | off | Use MiniMessage formatting in your own public chat messages (when allow-player-format is on). |
| `uxmessentials.communication.clearchat` | op | /clearchat (alias /chatclear) to flush the chat for online players. |
| `uxmessentials.communication.clearchat.exempt` | off | Keep your chat scrollback when staff run /clearchat. |
| `uxmessentials.communication.gui` | op | Open the communication admin panel (/communication gui and on the /uxmess gui hub). |
| `uxmessentials.communication.info.<page>` | everyone | Read one information page, such as /motd or /rules. |
| `uxmessentials.communication.info.info` | everyone | /info to read the shipped welcome / quick-start info page. |
| `uxmessentials.communication.info.motd` | everyone | /motd to read the shipped message-of-the-day info page. |
| `uxmessentials.communication.info.rules` | everyone | /rules to read the shipped server-rules info page. |
| `uxmessentials.communication.me` | everyone | /me to broadcast a third-person action message to all online players. |
| `uxmessentials.communication.togglechat` | op | /togglechat (alias /mutechat) to lock or unlock public chat for non-staff. |
| `uxmessentials.module.communication` | op | Hot-reload / inspect the communication module (connection messages, announcer, info pages). |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_communication_broadcasts%` | Whether the player receives the rotating announcements (yes/no). |
| `%uxmessentials_communication_chat_enabled%` | Whether public chat is open rather than locked by /togglechat (yes/no). |
{/* /generated */}

## Notes

- **The shipped chat format is near-vanilla on purpose,** so switching the module on changes nothing surprising
  until you author your own. `group-formats` overrides it per LuckPerms primary group.
- **A new info page gives you a command for free.** Define `staffguide` in `info-pages.conf` and `/staffguide`
  exists after a reload, gated by `uxmessentials.communication.info.staffguide`.
- **The announcer is edited in game.** `/announce editor` opens it, `/announce toggle` stops and starts the
  rotation, and `min-players` keeps it quiet on an empty server.
- **Chat control has two exemptions:** one lets a player's view survive `/clearchat`, the other lets staff keep
  speaking while `/togglechat` has public chat locked.
- **Achievement broadcasts are off by default.** `advancements.conf` decides which advancements are announced,
  in what wording, and to which channels.

Related: [Messaging](messaging.md), [Ranks](ranks.md), [Scoreboard](scoreboard.md)
