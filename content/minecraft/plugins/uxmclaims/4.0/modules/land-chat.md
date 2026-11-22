---
title: Land Chat
order: 1312
description: A private chat channel for the members of one land, with staff spying.
icon: message-square
---

Land chat lets the members of a land talk without the rest of the server reading along. Stand in the land and
type `/claim chat` to switch your chat to it, or `/claim chat <message>` to send one line. Staff with the spy
permission can read every land chat.

Module `land-chat` · enabled by default · `modules/land-chat/config.yml` · requires [Lands](lands.md)

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/claim chat` | Switches your chat to the land you stand in, or back to normal chat when it is already on. | `uxmclaims.command.land-chat.chat` |
| `/claim chat <message>` | Sends one message to the land you stand in, without switching modes. | `uxmclaims.command.land-chat.chat` |
| `/claim chat spy` | Turns reading every land chat on or off for you. | `uxmclaims.command.land-chat.spy` |

Both forms of `/claim chat` also need you to stand in the land and be its owner or a member with `land-chat:use`.
Players with `uxmclaims.admin` may use any land's chat. The local names of these commands are in
`modules/land-chat/lang/<language>/commands.yml`.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.command.land-chat.chat` | everyone | `/claim chat`. |
| `uxmclaims.command.land-chat.spy` | op | `/claim chat spy`, and receiving spied messages. |

| Role permission | Given to | Grants |
|---|---|---|
| `land-chat:use` | owner, member | Using the land's chat (3.x `CLAIM_CHAT`). |

## Settings

| Key | Default | What it does |
|---|---|---|
| `admin-spy-default` | `false` | Whether staff with the spy permission read every land chat until they turn it off with `/claim chat spy`. An upgrade from 3.x sets `true`, because 3.x always showed land chat to admins. |
| `recipients` | `channel` | Who gets a message. `channel`: members whose own chat mode is on for this land. `members`: every online member allowed to use land chat, chat mode or not. |

The message layout is in `modules/land-chat/lang/<language>/messages.yml`: `format` for members and
`spy-format` for spying staff. Both take `<land_name>`, `<player_name>` and `<message>`.

## Notes

- With `recipients: channel`, a member who has not typed `/claim chat` for that land does not see its messages,
  also not one-line `/claim chat <message>` messages. Use `members` if every member should always read along.
- Chat mode is held in memory: it ends when you leave the server, when the land is deleted, or when you lose
  `land-chat:use`.
- A spy switch set with `/claim chat spy` lasts until the staff member leaves the server; when they join again, `admin-spy-default` applies.
- Messages typed into a chat prompt (a menu asking for a name, for example) are never sent to the land.
- A typed `\n` is replaced by a space.

Related: [Lands](lands.md), [Group Chat](group-chat.md)
