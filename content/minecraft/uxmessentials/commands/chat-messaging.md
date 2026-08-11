---
title: Chat & Messaging
order: 1020
---

## Chat & Messaging

Two modules share this page. **Messaging** covers private conversations, offline mail, ignore lists, and staff support requests. **Communication** covers server-wide announcements, the rotating announcer, chat control, and info pages like `/rules` and `/motd`. Every command is Brigadier, so all of them can be renamed, re-aliased, or disabled in [`commands/commands.conf`](../config/commands-conf.md); admin-authored text (broadcasts, announcements, info pages) is MiniMessage-formatted.

---

## Messaging

| Command | Aliases | Description | Permission |
|---------|---------|-------------|------------|
| `/msg <player> <text>` | `m`, `tell`, `whisper`, `w` | Send a private message | `uxmessentials.msg.use` |
| `/reply <text>` | `r` | Reply to your last conversation | `uxmessentials.msg.reply` |
| `/mail [read\|send <player> <text>\|clear]` | - | Read, send, or clear persistent offline mail | `uxmessentials.mail.use` |
| `/mailclear` | - | Clear your own mailbox | `uxmessentials.mail.use` |
| `/mail sendall <text>` | - | Mail every online player (staff) | `uxmessentials.mail.sendall` |
| `/msgtoggle` | - | Refuse all incoming `/msg` and `/reply` | `uxmessentials.msg.toggle` |
| `/rtoggle` | - | Refuse `/reply` routing only | `uxmessentials.msg.toggle` |
| `/ignore [player]` | - | Ignore a player (bare form lists / opens the panel) | `uxmessentials.msg.ignore` |
| `/unignore <player>` | - | Stop ignoring a player | `uxmessentials.msg.ignore` |
| `/ignorelist` | - | Show who you are ignoring | `uxmessentials.msg.ignore` |
| `/socialspy [player]` | - | Watch other players' private messages (staff) | `uxmessentials.msg.socialspy` |
| `/helpop <text>` | - | Send a support request to staff | `uxmessentials.helpop.use` |
| `/msgsettings` | - | Open your personal messaging settings panel | `uxmessentials.msgsettings.use` |

Related nodes: `uxmessentials.helpop.receive` (see incoming `/helpop`), `uxmessentials.msg.color` (use color/formatting in messages), and `uxmessentials.msg.bypass.ignore` (message a player who is ignoring you).

<Callout type="info" title="Mail survives logout">

`/mail send` delivers even when the recipient is offline; they get it on next login. `/mail sendall` fans out to everyone currently online in one shot and is intended for staff.

</Callout>

### Examples

```
/msg Steve hey, meet me at spawn
/r on my way                     # reply to the last person
/mail send Alex thanks for the trade!
/mail read                       # open your mailbox
/ignore Griefer                  # stop seeing their messages
/msgtoggle                       # refuse all DMs
/helpop stuck in a wall at spawn
```

---

## Communication

Server-wide chat and announcements. The rotating **announcer** posts scheduled messages on a timer; `/announce` manages that rotation and `/broadcasttoggle` lets a player opt out of it. One-off `/broadcast` messages are separate from the rotation.

| Command | Aliases | Description | Permission |
|---------|---------|-------------|------------|
| `/broadcast <message>` | - | One-off announcement to everyone | `uxmessentials.communication.broadcast` |
| `/broadcastworld <message>` | `bcw` | Announce only to your current world | `uxmessentials.communication.broadcastworld` |
| `/broadcasttoggle` | - | Stop / resume seeing the rotating announcements | `uxmessentials.communication.broadcasttoggle` |
| `/announce [editor\|reload\|list\|preview <id>\|toggle]` | - | Manage the rotating announcer | `uxmessentials.announce.admin` |
| `/me <action>` | - | Third-person action message | `uxmessentials.communication.me` |
| `/clearchat` | `chatclear` | Flush public chat | `uxmessentials.communication.clearchat` |
| `/togglechat` | `mutechat` | Lock / unlock public chat for non-staff | `uxmessentials.communication.togglechat` |
| `/communication [gui]` | - | Communication admin panel | `uxmessentials.communication.gui` |

Related nodes: `uxmessentials.communication.clearchat.exempt` (your messages survive a `/clearchat`) and `uxmessentials.communication.chat.bypass` (talk while chat is toggled off).

### Info pages

Info pages are configured text screens. Each configured page **auto-registers its own `/<name> [page]` command** and its own gate node `uxmessentials.communication.info.<name>`. Three ship out of the box:

| Command | Permission |
|---------|------------|
| `/info [page]` | `uxmessentials.communication.info.info` |
| `/rules [page]` | `uxmessentials.communication.info.rules` |
| `/motd [page]` | `uxmessentials.communication.info.motd` |

<Callout type="tip" title="Add your own pages">

Define a new info page in the module config (say `discord`) and the plugin registers `/discord` plus `uxmessentials.communication.info.discord` automatically. Long pages paginate with `[page]`.

</Callout>

### Examples

```
/broadcast <yellow>Restart in 5 minutes!
/bcw Event starting in this world now
/me waves at everyone            # * Steve waves at everyone
/togglechat                      # lock chat for non-staff
/clearchat                       # wipe the chat window
/announce editor                 # open the announcer editor
/rules 2                         # read page 2 of the rules
```

---

## Next Steps

- [📣 Communication Feature Guide](../features/communication.md): the announcer, chat control, and info pages
- [📖 Command Overview](overview.md): how every uxmEssentials command is organised
- [🔐 Permissions Reference](../permissions/reference.md): full `uxmessentials.*` node list
- [⚙️ Renaming Commands](../config/commands-conf.md): rename, re-alias, or disable any command
