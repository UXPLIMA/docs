---
title: Chat bridge
order: 36
description: Game chat in one Discord channel and that channel's messages in game, and what each side can and cannot send.
icon: messages-square
---

The chat bridge puts game chat into one Discord channel, and the messages written in that channel
into game chat, so members can talk to players without joining. It is in `config.yml` and ships
turned off.

```yaml
chat-bridge:
  enabled: false
  discord-channel-id: "123456789012345678"
  minecraft-format: "**{player}**: {message}"
  discord-format: "<gradient:#5865F2:#7289DA>[Discord]</gradient> <white>{user}</white><dark_gray>:</dark_gray> <gray>{message}</gray>"
```

| Setting | Meaning |
|---|---|
| `discord-channel-id` | The one text channel that is bridged |
| `minecraft-format` | How game chat looks in Discord, with Discord formatting. `{player}` and `{message}` |
| `discord-format` | How Discord messages look in game, in MiniMessage. `{user}` and `{message}` |

Note the braces: the bridge uses `{player}`, not `%player%`. `/dsadmin reload` turns it on or off
without a restart.

## What crosses

| From the game | From Discord |
|---|---|
| Every chat message that reaches the chat. A message another plugin cancels, such as a muted player's, does not | Every message from a member with text in it, replies included |
| Not commands, joins, leaves or deaths | Not messages from bots, pins, boosts, or a picture without text |
| Shown in the channel by the bot | Shown to every player online |

`{user}` is the member's Discord username, not their nickname on your server.

## What cannot get through

| Side | Protection |
|---|---|
| Game to Discord | Discord formatting typed in game is escaped, so a player cannot hide a link, write a heading or a spoiler. Nobody can be pinged, not even with `<@id>` |
| Discord to game | `{message}` is inserted as plain text: MiniMessage tags typed in Discord, click actions, colour codes and line breaks do not work. A member cannot fake a server message |

The MiniMessage in `discord-format` applies to your format only, never to what a member typed.

<Callout type="warning" title="The bridge is a way into your game chat">

Anyone who can write in the bridged channel can put text in front of every player online. Let only
linked members write there, keep the channel out of public categories, and moderate it like game
chat, because now it is.

</Callout>

## Bridge or chat log

The bridge is a conversation; the [chat log](logging.md#chat-log) is a record of game chat posted
through a webhook. Do not point both at one channel: every game message would then show twice. Use a
bridged `#game-chat` for members and a logged `#chat-log` for staff.

## Several servers

Every server sends its own chat to the channel and shows the channel's messages to its own players.
The default format does not say which server a message came from. Put a fixed name in each server's
`minecraft-format`, such as `"[Survival] **{player}**: {message}"`.

Related: [Logging](logging.md), [Server stats and presence](server-stats.md)
