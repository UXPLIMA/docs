---
title: Player commands
order: 302
description: Every command a player has by default, with its arguments, its node and what it answers.
icon: user
---

Every node on this page is given to everyone by default. Take one from a rank to close the command to it.

## /chat

| Command | Node | What it does |
|---|---|---|
| `/chat` | `uxmchat.settings` | Opens the [settings window](../menus/settings.md) |
| `/chat settings` | `uxmchat.settings` | The same |
| `/chat lang` | `uxmchat.language` | Names the language you read and the ones there are |
| `/chat lang set <language>` | `uxmchat.language` | Reads this plugin in that language, such as `tr` or `pt-BR` |
| `/chat lang reset` | `uxmchat.language` | Goes back to the language of your client |

## /channel

| Command | Node | What it does |
|---|---|---|
| `/channel` | `uxmchat.channel.use` | Lists the channels you may use and the one you speak in |
| `/channel switch <channel>` | `uxmchat.channel.use` | Speaks in that channel from now on, and hears it |
| `/channel join <channel>` | `uxmchat.channel.use` | Hears that channel too, without speaking in it |
| `/channel leave <channel>` | `uxmchat.channel.use` | Stops hearing it |
| `/channel list` | `uxmchat.channel.use` | The channels, with which you hear and which you speak in |
| `/channel info <channel>` | `uxmchat.channel.use` | Its kind, scope, shortcut, marker, radius, slowmode and whether it is locked |

A channel's own `send-permission` and `read-permission` still apply. Shortcuts such as `/g` and markers such as `!`
are described in [Channels](../features/channels.md).

## /msg

| Command | Node | What it does |
|---|---|---|
| `/msg <player> <message...>` | `uxmchat.msg` | A private message to a player on any server of the network |
| `/reply <message...>` | `uxmchat.msg` | Answers whoever last messaged you, or whoever you last messaged |
| `/conversation [player]` | `uxmchat.msg.conversation` | Every line you type goes to that player, until you run it again with no name |
| `/quickreply <slot>` | `uxmchat.quickreply` | Sends the saved line in that slot as a reply |
| `/quickreply add <message...>` | `uxmchat.quickreply` | Saves a line in the next free slot, up to `quick-replies.cap` (9) |
| `/quickreply remove <slot>` | `uxmchat.quickreply` | Removes one |
| `/quickreply list` | `uxmchat.quickreply` | Your saved lines by slot |
| `/recent` | `uxmchat.recent` | The [recent lines window](../menus/recent.md): the last lines said to you, to answer or ignore |

`/msg` has the aliases `/tell`, `/w`, `/whisper`, `/m` and `/pm`; `/reply` has `/r`; `/conversation` has `/convo`;
`/quickreply` has `/qr`. A player who is offline gets the message when they join, up to
`private-messages.offline-queue` (20). See [Private messages](../features/private-messages.md).

## /ignore

| Command | Node | What it does |
|---|---|---|
| `/ignore <player> [what]` | `uxmchat.ignore` | Hides a player: `all` (the default), `chat`, `pm` or `mention` |
| `/ignore remove <player>` | `uxmchat.ignore` | Hears them again |
| `/ignore list` | `uxmchat.ignore` | Everyone you ignore, and what |
| `/unignore <player>` | `uxmchat.ignore` | The same as `/ignore remove` |

A player with `uxmchat.unignorable` cannot be ignored.

## /friend

| Command | Node | What it does |
|---|---|---|
| `/friend` | `uxmchat.friend` | Opens the [friends window](../menus/friends.md) |
| `/friend add <player>` | `uxmchat.friend` | Sends a request, which waits `friends.request-days` (7) |
| `/friend accept <player>` | `uxmchat.friend` | Accepts a request |
| `/friend deny <player>` | `uxmchat.friend` | Turns one down |
| `/friend remove <player>` | `uxmchat.friend` | Ends a friendship, on both sides |
| `/friend favourite <player>` | `uxmchat.friend` | Pins a friend to the top of the list, or unpins them |
| `/friend list` | `uxmchat.friend` | Your friends in chat, with the server each is on |
| `/friend requests` | `uxmchat.friend` | Requests waiting for your answer |

Aliases `/friends` and `/f`. A player may have `friends.cap` (100) friends.

## /mail

| Command | Node | What it does |
|---|---|---|
| `/mail` | `uxmchat.mail` | The first page of your letters |
| `/mail read [page]` | `uxmchat.mail` | One page, `mail.page-size` (8) letters |
| `/mail send <player> <message...>` | `uxmchat.mail.send` | Leaves a letter, for a player on any server or offline |
| `/mail clear` | `uxmchat.mail` | Empties your inbox |

## /tag

| Command | Node | What it does |
|---|---|---|
| `/tag` | `uxmchat.tag` | Opens the [tag window](../menus/tags.md) |
| `/tag list` | `uxmchat.tag` | The tags you hold, in chat |
| `/tag wear <tag> [variant]` | `uxmchat.tag` | Wears a tag you hold, or one of its variants |
| `/tag clear` | `uxmchat.tag` | Takes your tag off |
| `/tag buy <tag>` | `uxmchat.tag.buy` | Buys a tag that has a price |
| `/tag favourite <tag>` | `uxmchat.tag` | Pins it to the top of its category, or unpins it |
| `/tag search <text...>` | `uxmchat.tag` | The tags whose id or words hold the text |
| `/tag make <text...>` | `uxmchat.tag.make` | Writes a tag of your own, which staff approve first |

Alias `/tags`. Staff subcommands are on [Staff commands](staff.md#tag).

## /chatcolor

| Command | Node | What it does |
|---|---|---|
| `/chatcolor` | `uxmchat.colour` | Opens the [colour window](../menus/colours.md) |
| `/chatcolor set <colour>` | `uxmchat.colour` | A preset id, a named colour (with `uxmchat.colour.named`) or `#rrggbb` (with `uxmchat.colour.hex`) |
| `/chatcolor style <style>` | `uxmchat.colour` | Turns `bold`, `italic`, `underlined`, `strikethrough` or `obfuscated` on or off, each with its own node |
| `/chatcolor clear` | `uxmchat.colour` | Plain words again: no colour and no style |
| `/chatcolor list` | `uxmchat.colour` | The presets you may use |

Alias `/chatcolour`. Each preset needs `uxmchat.colour.preset.<id>`.

## /nick

| Command | Node | What it does |
|---|---|---|
| `/nick <nick>` | `uxmchat.nick` | Goes by that name in chat |
| `/nick off` | `uxmchat.nick` | Your own name again |

Alias `/nickname`. `uxmchat.nick` is not given by default. See [Nicknames](../features/nicks.md).

## /emoji

`/emoji` (aliases `/emojis`, node `uxmchat.emoji`) opens the [emoji window](../menus/emojis.md); a click puts the
emoji's shortcut in chat for you to copy.

## /me

`/me <action...>` (node `uxmchat.me`) says an action line in your channel, through the same gates and filters as chat.

## /quote

`/quote <line> <answer...>` (node `uxmchat.quote`) answers a line with a quote of it. It is what the quote button
after each line types for you; the line is the id the button carries.

## /chatsnapshot

`/chatsnapshot <id>` (node `uxmchat.snapshot.view`) opens an `[inv]`, `[ender]` or `[shulker]` snapshot. It is what
clicking the snapshot in chat runs, and it works from any server for `snapshots.keep-minutes` (30).

## /rules

| Command | Node | What it does |
|---|---|---|
| `/rules` | `uxmchat.rules` | Reads the rules in `rules.lines` |
| `/rules accept` | `uxmchat.rules` | Accepts them, so you may chat |

## /vote

`/vote <answer>` (node `uxmchat.vote`) answers the running poll with the answer's number, 1 to 20. It is what a
poll's buttons run.

## /helpop

`/helpop <question...>` (node `uxmchat.helpop`) asks the staff on every server. One question every
`helpop.cooldown-seconds` (60).

## /chatgame

| Command | Node | What it does |
|---|---|---|
| `/chatgame top` | `uxmchat.chatgame` | The winners' table |
| `/chatgame answer <token>` | `uxmchat.chatgame` | What clicking a reaction round runs |

Alias `/cg`. Every other round is answered by typing the answer in chat.

## /stafflist

`/stafflist` (alias `/staff`, node `uxmchat.stafflist`) lists the staff online on every server. Vanished staff are
listed only for those with `uxmchat.stafflist.vanished`.

## /clearchat self

`/clearchat self` (node `uxmchat.clearchat.self`) clears your own chat.

## Custom commands

`/report <player> <reason...>` ships with node `uxmchat.command.report` and a 60 second cooldown; `/discord` and
`/store` need nothing. See [Custom commands](../features/custom-commands.md).

Related: [Staff commands](staff.md), [Permissions](../permissions/reference.md#player)
