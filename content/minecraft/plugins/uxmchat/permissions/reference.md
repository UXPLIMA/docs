---
title: Reference
order: 701
description: Every one of the 109 permission nodes of uxmChat, with its default and what it allows.
icon: list-checks
---

Every node is declared in the plugin with a description and a default, so a permission plugin lists it. `everyone`
is `default: true`; `op` is operators only.

## Player

| Node | Default | Allows |
|---|---|---|
| `uxmchat.language` | everyone | Choose the language you read this plugin in |
| `uxmchat.settings` | everyone | Open your chat settings |
| `uxmchat.channel.use` | everyone | Switch, join and leave channels |
| `uxmchat.msg` | everyone | Send private messages and reply |
| `uxmchat.msg.conversation` | everyone | Talk to one player with every line you type |
| `uxmchat.ignore` | everyone | Ignore players |
| `uxmchat.friend` | everyone | Keep a friends list |
| `uxmchat.mail` | everyone | Read your letters |
| `uxmchat.mail.send` | everyone | Leave a letter for a player |
| `uxmchat.quickreply` | everyone | Save quick replies |
| `uxmchat.quote` | everyone | Answer a line with a quote |
| `uxmchat.recent` | everyone | Open the recent lines window |
| `uxmchat.rules` | everyone | Read and accept the rules |
| `uxmchat.snapshot.view` | everyone | Open inventory snapshots from chat |
| `uxmchat.emoji` | everyone | Use emoji and the emoji window |
| `uxmchat.colour` | everyone | Pick a chat colour |
| `uxmchat.tag` | everyone | Wear tags you hold |
| `uxmchat.tag.buy` | everyone | Buy tags |
| `uxmchat.tag.make` | everyone | Make a tag of your own, which staff approve |
| `uxmchat.me` | everyone | Use /me |
| `uxmchat.vote` | everyone | Vote in polls |
| `uxmchat.helpop` | everyone | Ask the staff |
| `uxmchat.chatgame` | everyone | Answer chat games and read the winners |
| `uxmchat.stafflist` | everyone | See which staff are online |
| `uxmchat.clearchat.self` | everyone | Clear your own chat |
| `uxmchat.command.report` | everyone | Run the /report custom command |

A player who lost one of these still sees its command and is told `command.no-permission`.

## Tokens

| Node | Default | Allows |
|---|---|---|
| `uxmchat.token.item` | everyone | Show the held item with [item] |
| `uxmchat.token.inv` | everyone | Show your inventory with [inv] |
| `uxmchat.token.ender` | everyone | Show your ender chest with [ender] |
| `uxmchat.token.shulker` | everyone | Show the held shulker box with [shulker] |
| `uxmchat.token.pos` | everyone | Show your position with [pos] |
| `uxmchat.token.money` | everyone | Show your balance with [money] |

## Colours and styles

| Node | Default | Allows |
|---|---|---|
| `uxmchat.colour.preset.white` | everyone | The white chat colour |
| `uxmchat.colour.preset.sky` | everyone | The sky chat colour |
| `uxmchat.colour.preset.mint` | everyone | The mint chat colour |
| `uxmchat.colour.preset.rose` | everyone | The rose chat colour |
| `uxmchat.colour.preset.butter` | everyone | The butter chat colour |
| `uxmchat.colour.preset.sunset` | op | The sunset gradient |
| `uxmchat.colour.preset.ocean` | op | The ocean gradient |
| `uxmchat.colour.preset.rainbow` | op | The rainbow colour |
| `uxmchat.colour.preset.candy` | op | The candy colour |
| `uxmchat.colour.preset.confetti` | op | The confetti colour |
| `uxmchat.colour.hex` | op | Type any #rrggbb chat colour |
| `uxmchat.colour.named` | op | Use any named chat colour |
| `uxmchat.colour.style.bold` | op | Bold chat words |
| `uxmchat.colour.style.italic` | op | Italic chat words |
| `uxmchat.colour.style.underlined` | op | Underlined chat words |
| `uxmchat.colour.style.strikethrough` | op | Struck chat words |
| `uxmchat.colour.style.obfuscated` | op | Scrambled chat words |

A preset you add needs `uxmchat.colour.preset.<id>`, or the `permission` it names. The gradient presets, hex and named colours and every style are operators only: give them to the ranks that may.

## Writing in chat

| Node | Default | Allows |
|---|---|---|
| `uxmchat.format.colour` | op | Write &-colour codes in chat |
| `uxmchat.format.hex` | op | Write hex colours in chat |
| `uxmchat.format.decoration` | op | Write bold, italic and the other decorations in chat |
| `uxmchat.format.magic` | op | Write obfuscated text in chat |
| `uxmchat.mention.here` | op | Use @here |
| `uxmchat.mention.everyone` | op | Use @everyone |
| `uxmchat.emoji.faces` | op | The face emoji |
| `uxmchat.emoji.seasonal` | op | The seasonal emoji |
| `uxmchat.nick` | op | Choose a nickname |
| `uxmchat.nick.others` | op | Set another player's nickname |
| `uxmchat.nick.realname` | op | Find who goes by a nickname |

`uxmchat.nick` is operators only by default: give it to the ranks that may choose a nickname.

## Channels

| Node | Default | Allows |
|---|---|---|
| `uxmchat.channel.staff` | op | Speak in and hear the staff channel |
| `uxmchat.channel.lock` | op | Lock and unlock channels |
| `uxmchat.channel.slowmode` | op | Set a channel's slowmode |
| `uxmchat.channel.force` | op | Move a player into a channel |
| `uxmchat.channel.kick` | op | Take a player out of a channel |

A channel's own `send-permission` and `read-permission` are checked as written in the channel; the shipped `staff` channel uses `uxmchat.channel.staff`.

## Staff

| Node | Default | Allows |
|---|---|---|
| `uxmchat.staff` | op | Count as staff: staff notices, the staff list, network moves |
| `uxmchat.staff.listen` | op | Hear channels that staff listen to without joining |
| `uxmchat.staff.filter` | op | Be told about every filter catch |
| `uxmchat.staff.helpop` | op | Read helpop questions |
| `uxmchat.staff.mute` | op | Be told about mutes |
| `uxmchat.staff.tags` | op | Be told about tags waiting for approval |
| `uxmchat.staff.delete` | op | Remove a line for everyone |
| `uxmchat.stafflist.vanished` | op | See vanished staff in the staff list and their moves |
| `uxmchat.see-vanished` | op | See and message vanished players |
| `uxmchat.unignorable` | op | Never be ignored |
| `uxmchat.mute` | op | Mute and unmute players |
| `uxmchat.broadcast` | op | Broadcast to every server |
| `uxmchat.bossbar` | op | Show a boss bar to players |
| `uxmchat.clearchat` | op | Clear the chat |
| `uxmchat.chatlock` | op | Close and open the chat |
| `uxmchat.chatlog` | op | Read the chat log |
| `uxmchat.audit` | op | Read the staff action log |
| `uxmchat.poll` | op | Start and end polls |
| `uxmchat.macro` | op | Say staff macros |
| `uxmchat.chatgame.start` | op | Start a chat game round |
| `uxmchat.tag.give` | op | Give, take, approve and turn down tags |
| `uxmchat.spy.pm` | op | Read copies of private messages |
| `uxmchat.spy.command` | op | Read copies of commands |
| `uxmchat.spy.surface` | op | Read copies of signs, books and item names |
| `uxmchat.editor.channels` | op | Edit channels in game |
| `uxmchat.editor.tags` | op | Edit tags in game |
| `uxmchat.editor.announcements` | op | Edit announcements in game |
| `uxmchat.editor.actions` | op | Edit tag action lines, which can run console commands |

`uxmchat.staff` makes a player staff: they appear in `/stafflist` and are told when staff move. The `uxmchat.staff.<kind>` nodes decide which notices a staff member reads; each is granted on its own.

## Bypass

| Node | Default | Allows |
|---|---|---|
| `uxmchat.bypass.filter` | op | Pass every filter |
| `uxmchat.bypass.slowmode` | op | Pass every slowmode |
| `uxmchat.bypass.length` | op | Write lines over the length limit |
| `uxmchat.bypass.rules` | op | Chat without accepting the rules |
| `uxmchat.bypass.channel-lock` | op | Speak in locked channels |
| `uxmchat.bypass.lock` | op | Speak while the chat is closed |
| `uxmchat.bypass.clear` | op | Keep your chat when it is cleared |
| `uxmchat.bypass.ai` | op | Skip the AI review |
| `uxmchat.bypass.mail-cap` | op | Send to a full inbox |
| `uxmchat.bypass.pm-cooldown` | op | Send private messages without waiting |
| `uxmchat.bypass.pm-toggle` | op | Message players who turned messages off |

## Admin

| Node | Default | Allows |
|---|---|---|
| `uxmchat.admin.doctor` | op | Ask how this plugin is |
| `uxmchat.admin.version` | op | Read the version this server runs |
| `uxmchat.admin.reload` | op | Read the configuration again |
| `uxmchat.admin.language` | op | Set or clear the language another player reads |
| `uxmchat.admin.*` | op | Everything staff and operators do with this plugin |

`uxmchat.admin.*` lists every `op` node of this page as a child, so granting it gives the whole staff and admin set at once.

## Per item

| Node | Gives |
|---|---|
| `uxmchat.tag.<id>` | One tag, unless the tag names its own `permission` or is sold |
| `uxmchat.colour.preset.<id>` | One colour preset |
| `uxmchat.colour.style.<name>` | One style |
| a channel's `send-permission`, `read-permission`, `auto-join-permission` | Speaking in, hearing and joining that channel |
| an emoji's `permission` | That emoji |
| a custom command's `permission` | That command |

## Examples

```
lp group default permission set uxmchat.nick false
lp group vip permission set uxmchat.colour.preset.sunset true
lp group vip permission set uxmchat.nick true
lp group helper permission set uxmchat.staff true
lp group helper permission set uxmchat.staff.helpop true
lp group helper permission set uxmchat.channel.staff true
lp group moderator permission set uxmchat.mute true
```

Related: [Commands](../commands/overview.md)
