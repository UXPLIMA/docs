---
title: Friends, ignore and mail
order: 411
description: A friends list across the network, ignoring what a player says, and letters to players who are away.
icon: users
---

## Friends

Module: `friends`. `/friend add <player>` sends a request with clickable `[accept]` and `[deny]`; it waits
`friends.request-days` (7). Friends see where each other are, and a player who turned off messages from strangers
still gets them from friends.

| Key under `friends` | Default | Meaning |
|---|---|---|
| `cap` | `100` | The most friends one player may have |
| `request-days` | `7` | How long a request waits for an answer |

`/friend` opens the [friends window](../menus/friends.md): a left click asks for a line and sends it to the friend in
private, a right click pins them to the top. `/friend list` lists them in chat with the server each is on.

## Ignore

Module: `ignore`. `/ignore <player> [what]` hides a player:

| What | Hides |
|---|---|
| `all` (the default) | Everything below |
| `chat` | Their public lines |
| `pm` | Their private messages |
| `mention` | Their mentions of you |

`/ignore list` shows who and what, `/ignore remove` or `/unignore` undoes it. A player with `uxmchat.unignorable`
cannot be ignored.

## Mail

Module: `mail`. `/mail send <player> <message>` leaves a letter for a player on any server or offline; at their join
they are told how many are unread, with a `[read]` link.

| Key under `mail` | Default | Meaning |
|---|---|---|
| `inbox-cap` | `50` | The most letters one inbox holds; `uxmchat.bypass.mail-cap` sends to a full one |
| `page-size` | `8` | Letters per page of `/mail read` |
| `retention-days` | `60` | How long a read letter is kept |

A letter passes the chat filters. Node `uxmchat.mail` reads and clears, `uxmchat.mail.send` sends.

Related: [Private messages](private-messages.md), [Player commands](../commands/player.md#friend)
