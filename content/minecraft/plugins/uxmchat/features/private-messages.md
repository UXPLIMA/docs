---
title: Private messages
order: 403
description: /msg across the network, offline delivery, replies, conversations, quick replies, the recent lines window and who may message whom.
icon: mail
---

Module: `private-messages`. Off, `/msg`, `/reply`, `/conversation`, `/quickreply` and `/recent` are gone.

## Sending

`/msg <player> <message>` reaches a player on any server of the network. The sender reads
`[me -> Name] message`, the receiver `[Name -> me] message`.

| Case | What happens |
|---|---|
| The receiver is offline | The message waits and is delivered at their join, marked `[while away]`; up to `offline-queue` (20) wait, the oldest goes first |
| The receiver turned private messages off | Refused, unless the sender has `uxmchat.bypass.pm-toggle` |
| The receiver turned off messages from strangers | Only their friends get through |
| The receiver ignores the sender | Refused |
| The receiver is vanished | Treated as offline, unless the sender has `uxmchat.see-vanished` |
| Too soon after the last | Refused for `cooldown-millis` (1000), unless the sender has `uxmchat.bypass.pm-cooldown` |
| Too long | Over `max-length` (256) characters is refused |

A message passes the same [filters](filters.md) as chat. Staff with `uxmchat.spy.pm` and spy turned on in their
settings read a copy of every message.

## Replying

`/reply <message>` (alias `/r`) answers whoever last messaged you, or whoever you last messaged.

## Conversations

`/conversation <player>` (alias `/convo`) sends every line you type to that player until you run `/conversation`
again with no name. Node `uxmchat.msg.conversation`.

## Quick replies

A player saves up to `quick-replies.cap` (9) lines with `/quickreply add <message>` and sends one with
`/quickreply <slot>` as a reply. `/quickreply list` shows them by slot.

## The recent lines window

`/recent` opens the last five lines said to you. A left click asks for a line and sends it in private to whoever said
the clicked one; a right click ignores them. A Bedrock player gets it as a form. See [Recent](../menus/recent.md).

## Keys

`private-messages` in [config.conf](../config/config-conf.md#private-messages-friends-mail-quick-replies-nicks):

| Key | Default | Meaning |
|---|---|---|
| `max-length` | `256` | The longest message, in characters |
| `cooldown-millis` | `1000` | The least time between two messages from one player |
| `offline-queue` | `20` | How many messages wait for one offline player |

Related: [Friends, ignore and mail](friends-ignore-mail.md), [Player commands](../commands/player.md#msg)
