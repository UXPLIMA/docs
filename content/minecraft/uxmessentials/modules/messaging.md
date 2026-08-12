---
title: Messaging
order: 9110
description: Private messages, offline mail, ignore lists, social spy and support requests.
---

Messaging covers what one player says to another rather than to the room: `/msg` and `/reply`, mail that waits
for an offline player, per-player ignore lists, the staff social spy, and `/helpop` support requests. Each
player carries their own switches, reachable from `/msgsettings`.

Module `messaging` · enabled by default · `modules/messaging/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/helpop` | Open a staff support request. | `uxmessentials.helpop.use` |
| `/ignore` | Ignore a player's messages, or open your ignore list. | `uxmessentials.msg.ignore` |
| `/ignorelist` | Open your ignore list. | `uxmessentials.msg.ignore` |
| `/mail` | Open your mailbox, or read, send or clear your persistent mail. | `uxmessentials.mail.use` |
| `/mailclear` | Clear your mailbox. | `uxmessentials.mail.use` |
| `/msg` (`/m`, `/tell`, `/whisper`, `/w`) | Send a private message to a player. | `uxmessentials.msg.use` |
| `/msgsettings` | Open your personal messaging settings panel. | `uxmessentials.msgsettings.use` |
| `/msgtoggle` | Refuse or accept incoming private messages. | `uxmessentials.msg.toggle` |
| `/reply` (`/r`) | Reply to your last private conversation. | `uxmessentials.msg.reply` |
| `/rtoggle` | Toggle whether you receive replies. | `uxmessentials.msg.toggle` |
| `/socialspy` | Observe other players' private messages, globally or for one player. | `uxmessentials.msg.socialspy` |
| `/unignore` | Stop ignoring a player's messages. | `uxmessentials.msg.ignore` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.helpop.receive` | op | Receive /helpop requests (staff side). |
| `uxmessentials.helpop.use` | everyone | /helpop \<text> to open a player-to-staff support request. |
| `uxmessentials.mail.sendall` | op | /mail sendall \<text> to broadcast mail to every online player (staff). |
| `uxmessentials.mail.use` | everyone | /mail read / /mail send \<player> \<text> / /mail clear (/mailclear): persistent offline mail. |
| `uxmessentials.messaging.gui` | op | See and open the messaging settings panel and mailbox on the /uxmess gui management hub. |
| `uxmessentials.module.messaging` | op | Hot-reload / inspect the messaging module (private messages, mail and social spy). |
| `uxmessentials.msg.color` | op | Render MiniMessage tags in PM/mail bodies (default plain text). |
| `uxmessentials.msg.ignore` | everyone | /ignore \<player> / /unignore \<player> / /ignorelist to manage and view your own ignore list. |
| `uxmessentials.msg.reply` | everyone | /reply \<text> to answer your last conversation (reply-TTL bounded). |
| `uxmessentials.msg.socialspy` | op | /socialspy to observe other players' private messages (staff). |
| `uxmessentials.msg.toggle` | everyone | /msgtoggle to refuse incoming /msg / /reply; /rtoggle to refuse only incoming /reply routing (mail still delivers). |
| `uxmessentials.msg.use` | everyone | /msg \<player> \<text> to send a private message. |
| `uxmessentials.msgsettings.use` | everyone | /msgsettings opens your personal messaging settings panel (accept-messages, social spy). |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `reply-ttl-seconds` | `300` | window in which /r still resolves to the last partner |
| `mail-retention-days` | `30` | age at which the sweep deletes mail |
| `mail-sweep-minutes` | `30` | how often the mail-expiry sweep runs |
| `offline-to-mail` | `true` | /msg to an offline player stores it as mail instead of failing |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_ignored_by%` | Whether the player the line is about ignores the viewer (yes/no). |
| `%uxmessentials_ignoring%` | Whether the viewer's ignore list holds the player the line is about (yes/no). |
| `%uxmessentials_messaging_ignoring_count%` | How many players this player is ignoring. |
| `%uxmessentials_messaging_mail_total%` | How much mail the player holds. |
| `%uxmessentials_messaging_mail_unread%` | How much mail the player has not read. |
| `%uxmessentials_messaging_msgtoggle%` | Whether the player accepts private messages (yes/no). |
| `%uxmessentials_messaging_reply_target%` | Who /r would answer: the last player this one talked to. |
| `%uxmessentials_messaging_socialspy%` | Whether the player is watching other players' messages (yes/no). |
{/* /generated */}

## Notes

- **`/msgtoggle` and `/rtoggle` are different refusals.** The first turns away every incoming message, the
  second only stops `/reply` routing back to you.
- **Mail waits in the database,** so a message sent to an offline player is still there after a restart or a
  world rollback, and `/mail sendall` reaches everyone at once.
- **Ignoring is the player's own tool.** A bare `/ignore` lists who they ignore and opens the panel; staff are
  not exempt from it unless you grant the bypass.
- **Social spy shows staff other players' conversations,** which is an investigation tool: grant it narrowly.
- **A vanished player is not reachable** by anyone whose see level does not cover them, because messaging reads
  the vanish module rather than keeping its own idea of who is online.

Related: [Communication](communication.md), [Vanish](vanish.md), [Discord Link](discordlink.md)
