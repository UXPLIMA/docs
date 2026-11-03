---
title: "Reference"
order: 20
description: "Every command, permission, placeholder and file of uxmChat."
---

## Commands

Names and aliases are the defaults from `commands.conf`, which renames or disables each of them.

| Command | Aliases | Subcommands | What it does |
|---|---|---|---|
| `/bossbar` |  |  | Show a bar to players |
| `/broadcast` | `/bc` |  | Say something to everyone |
| `/channel` | `/ch` | `switch`, `join`, `leave`, `list`, `info`, `lock`, `unlock`, `slowmode`, `force`, `kick` | Choose where you speak and what you hear |
| `/chat` |  | `settings`, `lang`, `lang set`, `lang reset` | Your chat: settings and language |
| `/chatcolor` | `/chatcolour` | `set`, `style`, `clear`, `list` | The colour of your words |
| `/chatdelete` |  |  | Remove a line |
| `/chateditor` |  | `channels`, `tags`, `announcements`, `announcement` | Edit channels, tags, announcement bundles and each announcement |
| `/announcements` |  | `preview`, `say` | List, preview and say announcements |
| `/chatgame` | `/cg` | `top`, `answer`, `start` | The chat games |
| `/chatlock` | `/muteall`, `/lockchat` |  | Close or open the chat |
| `/chatlog` |  | `player`, `search`, `audit` | Read what was said |
| `/chatsnapshot` |  |  | Look inside a snapshot |
| `/clearchat` |  | `self` | Clear the chat |
| `/conversation` | `/convo` |  | Talk to one player with every line you type |
| `/emoji` | `/emojis` |  | Pick an emoji to copy |
| `/friend` | `/friends`, `/f` | `add`, `accept`, `deny`, `remove`, `favourite`, `list`, `requests` | Your friends on the network |
| `/helpop` |  |  | Ask the staff |
| `/ignore` |  | `remove`, `list` | Stop hearing a player |
| `/macro` |  |  | Say a saved staff line |
| `/mail` |  | `read`, `send`, `clear` | Letters to and from players |
| `/me` |  |  | Describe what you do |
| `/msg` | `/tell`, `/w`, `/whisper`, `/m`, `/pm` |  | Send a private message |
| `/mute` |  |  | Stop a player from talking (absent when a punishment plugin keeps mutes) |
| `/nick` | `/nickname` | `off`, `player`, `realname` | Choose the name you go by in chat |
| `/poll` |  | `start`, `end` | Ask the server a question |
| `/quickreply` | `/qr` | `add`, `remove`, `list` | Saved replies |
| `/quote` |  |  | Answer a line with a quote |
| `/recent` |  |  | The last lines said to you |
| `/reply` | `/r` |  | Answer your last private message |
| `/rules` |  | `accept` | The server rules |
| `/stafflist` | `/staff` |  | Staff online on the network |
| `/tag` | `/tags` | `list`, `wear`, `clear`, `buy`, `favourite`, `search`, `make`, `pending`, `approve`, `deny`, `give`, `take` | The tag in front of your name |
| `/unignore` |  |  | Hear a player again |
| `/unmute` |  |  | Let a player talk again (absent when a punishment plugin keeps mutes) |
| `/uxmchat` | `/chatadmin` | `reload`, `version`, `doctor`, `lang set-player`, `lang reset-player`, `help` | Administer the plugin |
| `/vote` |  |  | Answer the running poll |

Channel shortcuts (`/g`, `/l`, `/w`, `/trade`, `/help-chat`, `/sc`) come from `channels.conf`, and commands of your own from `modules/custom-commands/custom-commands.conf`.

## Permissions

| Permission | What it allows | Default |
|---|---|---|
| `uxmchat.language` | Choose the language you read this plugin in | everyone |
| `uxmchat.settings` | Open your chat settings | everyone |
| `uxmchat.channel.use` | Switch, join and leave channels | everyone |
| `uxmchat.msg` | Send private messages and reply | everyone |
| `uxmchat.msg.conversation` | Talk to one player with every line you type | everyone |
| `uxmchat.ignore` | Ignore players | everyone |
| `uxmchat.friend` | Keep a friends list | everyone |
| `uxmchat.mail` | Read your letters | everyone |
| `uxmchat.mail.send` | Leave a letter for a player | everyone |
| `uxmchat.quickreply` | Save quick replies | everyone |
| `uxmchat.quote` | Answer a line with a quote | everyone |
| `uxmchat.recent` | Open the recent lines window | everyone |
| `uxmchat.rules` | Read and accept the rules | everyone |
| `uxmchat.snapshot.view` | Open inventory snapshots from chat | everyone |
| `uxmchat.emoji` | Use emoji and the emoji window | everyone |
| `uxmchat.colour` | Pick a chat colour | everyone |
| `uxmchat.tag` | Wear tags you hold | everyone |
| `uxmchat.tag.buy` | Buy tags | everyone |
| `uxmchat.tag.make` | Make a tag of your own, which staff approve | everyone |
| `uxmchat.me` | Use /me | everyone |
| `uxmchat.vote` | Vote in polls | everyone |
| `uxmchat.helpop` | Ask the staff | everyone |
| `uxmchat.chatgame` | Answer chat games and read the winners | everyone |
| `uxmchat.stafflist` | See which staff are online | everyone |
| `uxmchat.clearchat.self` | Clear your own chat | everyone |
| `uxmchat.command.report` | Run the /report custom command | everyone |
| `uxmchat.token.item` | Show the held item with [item] | everyone |
| `uxmchat.token.inv` | Show your inventory with [inv] | everyone |
| `uxmchat.token.ender` | Show your ender chest with [ender] | everyone |
| `uxmchat.token.shulker` | Show the held shulker box with [shulker] | everyone |
| `uxmchat.token.pos` | Show your position with [pos] | everyone |
| `uxmchat.token.money` | Show your balance with [money] | everyone |
| `uxmchat.colour.preset.white` | The white chat colour | everyone |
| `uxmchat.colour.preset.sky` | The sky chat colour | everyone |
| `uxmchat.colour.preset.mint` | The mint chat colour | everyone |
| `uxmchat.colour.preset.rose` | The rose chat colour | everyone |
| `uxmchat.colour.preset.butter` | The butter chat colour | everyone |
| `uxmchat.colour.preset.sunset` | The sunset gradient | op |
| `uxmchat.colour.preset.ocean` | The ocean gradient | op |
| `uxmchat.colour.preset.rainbow` | The rainbow colour | op |
| `uxmchat.colour.preset.candy` | The candy colour | op |
| `uxmchat.colour.preset.confetti` | The confetti colour | op |
| `uxmchat.colour.hex` | Type any #rrggbb chat colour | op |
| `uxmchat.colour.named` | Use any named chat colour | op |
| `uxmchat.colour.style.bold` | Bold chat words | op |
| `uxmchat.colour.style.italic` | Italic chat words | op |
| `uxmchat.colour.style.underlined` | Underlined chat words | op |
| `uxmchat.colour.style.strikethrough` | Struck chat words | op |
| `uxmchat.colour.style.obfuscated` | Scrambled chat words | op |
| `uxmchat.format.colour` | Write &-colour codes in chat | op |
| `uxmchat.format.hex` | Write hex colours in chat | op |
| `uxmchat.format.decoration` | Write bold, italic and the other decorations in chat | op |
| `uxmchat.format.magic` | Write obfuscated text in chat | op |
| `uxmchat.mention.here` | Use @here | op |
| `uxmchat.mention.everyone` | Use @everyone | op |
| `uxmchat.emoji.faces` | The face emoji | op |
| `uxmchat.emoji.seasonal` | The seasonal emoji | op |
| `uxmchat.nick` | Choose a nickname | op |
| `uxmchat.nick.others` | Set another player's nickname | op |
| `uxmchat.nick.realname` | Find who goes by a nickname | op |
| `uxmchat.channel.staff` | Speak in and hear the staff channel | op |
| `uxmchat.channel.lock` | Lock and unlock channels | op |
| `uxmchat.channel.slowmode` | Set a channel's slowmode | op |
| `uxmchat.channel.force` | Move a player into a channel | op |
| `uxmchat.channel.kick` | Take a player out of a channel | op |
| `uxmchat.staff` | Count as staff: staff notices, the staff list, network moves | op |
| `uxmchat.staff.listen` | Hear channels that staff listen to without joining | op |
| `uxmchat.staff.filter` | Be told about every filter catch | op |
| `uxmchat.staff.helpop` | Read helpop questions | op |
| `uxmchat.staff.mute` | Be told about mutes | op |
| `uxmchat.staff.tags` | Be told about tags waiting for approval | op |
| `uxmchat.staff.delete` | Remove a line for everyone | op |
| `uxmchat.stafflist.vanished` | See vanished staff in the staff list and their moves | op |
| `uxmchat.see-vanished` | See and message vanished players | op |
| `uxmchat.unignorable` | Never be ignored | op |
| `uxmchat.mute` | Mute and unmute players | op |
| `uxmchat.broadcast` | Broadcast to every server | op |
| `uxmchat.bossbar` | Show a boss bar to players | op |
| `uxmchat.clearchat` | Clear the chat | op |
| `uxmchat.chatlock` | Close and open the chat | op |
| `uxmchat.chatlog` | Read the chat log | op |
| `uxmchat.audit` | Read the staff action log | op |
| `uxmchat.poll` | Start and end polls | op |
| `uxmchat.macro` | Say staff macros | op |
| `uxmchat.chatgame.start` | Start a chat game round | op |
| `uxmchat.tag.give` | Give, take, approve and turn down tags | op |
| `uxmchat.spy.pm` | Read copies of private messages | op |
| `uxmchat.spy.command` | Read copies of commands | op |
| `uxmchat.spy.surface` | Read copies of signs, books and item names | op |
| `uxmchat.editor.channels` | Edit channels in game | op |
| `uxmchat.editor.tags` | Edit tags in game | op |
| `uxmchat.editor.announcements` | Edit, list, preview and say announcements | op |
| `uxmchat.editor.actions` | Edit tag action lines, which can run console commands | op |
| `uxmchat.bypass.filter` | Pass every filter | op |
| `uxmchat.bypass.slowmode` | Pass every slowmode | op |
| `uxmchat.bypass.length` | Write lines over the length limit | op |
| `uxmchat.bypass.rules` | Chat without accepting the rules | op |
| `uxmchat.bypass.channel-lock` | Speak in locked channels | op |
| `uxmchat.bypass.lock` | Speak while the chat is closed | op |
| `uxmchat.bypass.clear` | Keep your chat when it is cleared | op |
| `uxmchat.bypass.ai` | Skip the AI review | op |
| `uxmchat.bypass.mail-cap` | Send to a full inbox | op |
| `uxmchat.bypass.pm-cooldown` | Send private messages without waiting | op |
| `uxmchat.bypass.pm-toggle` | Message players who turned messages off | op |
| `uxmchat.admin.doctor` | Ask how this plugin is | op |
| `uxmchat.admin.version` | Read the version this server runs | op |
| `uxmchat.admin.reload` | Read the configuration again | op |
| `uxmchat.admin.language` | Set or clear the language another player reads | op |
| `uxmchat.admin.*` | Everything staff and operators do with this plugin | op |

Per-item nodes: `uxmchat.tag.<id>` holds a tag, `uxmchat.colour.preset.<id>` a colour preset, `uxmchat.colour.style.<name>` a style, and a channel's own `send-permission` and `read-permission`.

## Placeholders

Formats read PlaceholderAPI placeholders when PlaceholderAPI is installed, and uxmChat answers its own for
tab lists, scoreboards and other plugins. Coloured values come with section codes and hex colours, and the
`_mini` forms with MiniMessage for a plugin that parses it; a player who is not online on that server reads as
empty, except `%uxmchat_server%`.

A tag or a channel name written as a language key reads in the language of the player it is about. Every
placeholder also answers as `%rel_uxmchat_<name>%`, which reads it in the language of the player the chat line,
tab row or nametag is drawn for: `%rel_uxmchat_tag_mini%` in another plugin's nametag shows each viewer the tag
in their own language.

| Placeholder | Value |
|---|---|
| `%uxmchat_channel%` | The id of the channel the player talks in |
| `%uxmchat_channel_name%` | That channel's coloured name |
| `%uxmchat_channel_name_mini%` | That name as MiniMessage |
| `%uxmchat_tag%` | The coloured tag the player shows, or empty |
| `%uxmchat_tag_mini%` | That tag as MiniMessage, or empty |
| `%uxmchat_tag_plain%` | That tag without formatting, or empty |
| `%uxmchat_has_tag%` | Whether the player shows a tag: `true` or `false` |
| `%uxmchat_tag_id%` | The id of the tag the player chose, or empty |
| `%uxmchat_nick%` | The coloured nick, or the name |
| `%uxmchat_colour%` | The id of the chat colour, or empty; `%uxmchat_color%` too |
| `%uxmchat_muted%` | `true` or `false` |
| `%uxmchat_messages%` | Whether private messages are on: `true` or `false` |
| `%uxmchat_server%` | The server the player is on in the network |
| `%uxmchat_online%` | How many players the network shows online |

## API

Other plugins reach `com.uxplima.chat.api.UxmChat` through Bukkit's services manager and listen to the events in `com.uxplima.chat.api.event`: `ChatLineEvent` (cancellable), `ChatLineSentEvent`, `ChatMentionEvent`, `PrivateMessageEvent`, `ServerLineEvent` and `StaffActionEvent`.
