---
title: Group Chat
order: 1337
description: Town, nation and ally chat channels, sent once or kept as your chat focus.
icon: messages-square
---

Group chat gives towns, nations and their allies their own chat channels. Type `/tc hello` to send one line to
your town, or `/tc` alone to make every following chat line go there until you type it again. Global chat is
left to your chat plugin.

Module `group-chat` · disabled by default · `modules/group-chat/config.yml` · requires [Towns](towns.md)

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/tc [message]`, `/town chat [message]` | Sends a line to your town, or switches your chat focus to town chat and back. | none |
| `/nc [message]`, `/nation chat [message]` | The same for your nation. | none |
| `/ac [message]`, `/town allychat [message]`, `/nation allychat [message]` | The same for your nation (or town) and every holder allied with it. | none |
| `/uxm chat spy` | Turns spying on every town, nation and ally chat on or off for you. | `uxmclaims.command.group-chat.spy` |

Local names and aliases for these commands go in `lang/<code>/commands.yml` of the module and need a restart.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.command.group-chat.spy` | op | `/uxm chat spy`, and receiving group chat lines of groups you are not in while spying is on. |

## Settings

| Key | Default | What it does |
|---|---|---|
| `channels.town` | `true` | Allows town chat. |
| `channels.nation` | `true` | Allows nation chat. Needs the [Nations](nations.md) module. |
| `channels.ally` | `true` | Allows ally chat. Needs the [Relations](relations.md) module. |
| `admin-spy-default` | `false` | Whether staff with the spy node see every group chat until they turn it off with `/uxm chat spy`. |
| `keep-focus` | `true` | Whether a chat focus survives a rejoin or restart. `false` ends the focus when the player leaves. |

The line formats are in `lang/<code>/messages.yml` of the module: `format.town`, `format.nation`, `format.ally`
and `spy-format`, with the tags `<group>`, `<player_name>` and `<message>`.

## Who receives a line

| Channel | Reaches |
|---|---|
| Town | Every online resident of your town. |
| Nation | Every online resident of every town (and land) in your nation. |
| Ally | Your nation and your town, plus every town, nation or land in a live ally relation with either. The channel name shown is your nation's, or your town's when you have no nation. |

Spying staff outside the group get the line in `spy-format`. A player focused on a channel who leaves the group,
or whose channel is turned off, drops back to global chat and their line goes there.

## Notes

- Group chat listens at LOW priority and skips cancelled chat lines, so answers to chat prompts and lines your chat plugin cancels are left alone. Players with no focus are never touched.
- The focus and the spy switch are stored in the player's preferences, not in a table of this module.
- A Discord bridge or another plugin can relay lines into a channel through the API. Every line, from the game or relayed, fires a group chat message event.
- There is no local (range) channel yet, and add-ons cannot register channels of their own yet.

Related: [Towns](towns.md), [Nations](nations.md), [Relations](relations.md), [Land Chat](land-chat.md), [Discord](discord.md)
