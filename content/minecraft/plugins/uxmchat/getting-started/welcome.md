---
title: Welcome
order: 101
description: What uxmChat does, who it is for, and where to read next.
icon: hand
---

uxmChat is the whole chat of a server, or of every server behind one Velocity proxy. Channels decide who hears a
line, formats decide how it looks, filters decide whether it goes out at all, and around them sit private messages,
friends, mail, tags, chat colours, announcements, chat games and the staff tools that keep it all in order.

Every feature is a module. A module turned off registers no listener, task, command or placeholder, and a reload
turns it on or off without a restart, so a server that only wants channels and private messages runs exactly that.

## What it does

| Part | What it gives you |
|---|---|
| [Channels](../features/channels.md) | Global, local and world channels, on one server or the whole network, with shortcuts and markers |
| [Formats](../features/formats.md) | A line built from parts, each with its own hover and click, per channel and per group |
| [Private messages](../features/private-messages.md) | `/msg` across the network, offline delivery, conversations, quick replies, the recent lines window |
| [Mentions and tokens](../features/mentions-and-tokens.md) | `@name` with a sound, `[item]`, `[inv]`, `[pos]`, `[money]` and the other tokens |
| [Filters](../features/filters.md) | Swearing, adverts, spam, caps, flood, and a ladder that mutes repeat offenders |
| [Mutes](../features/mutes.md) | uxmChat's own mutes, or the ones of the punishment plugin you already run |
| [Emoji](../features/emojis.md) | `:heart:` and friends, with a window to pick from |
| [Chat colours](../features/colours.md) | Solid colours, gradients, rainbow and styles for a player's own words |
| [Tags](../features/tags.md) | A word before the name: given by rank, bought, animated, or written by the player |
| [Nicknames](../features/nicks.md) | The name a player goes by in chat |
| [Friends, ignore and mail](../features/friends-ignore-mail.md) | A friends list across the network, ignoring, letters to offline players |
| [Announcements](../features/announcements.md) | Bundles of notices on chat, action bar, title, boss bar or sound |
| [Server lines](../features/events.md) | Join, quit, death, kick, advancement and unknown command lines |
| [Custom commands](../features/custom-commands.md) | Commands of your own, with conditions and actions |
| [Chat games](../features/games.md) | Seven kinds of round that the first right answer wins |
| [Staff tools](../features/staff-tools.md) | Spy, chat log, delete, lock, clear, broadcast, boss bar, polls, rules, helpop, macros, staff list |
| [Bubbles and newcomers](../features/bubbles-and-newcomers.md) | Words over heads, and gentler limits for new players |
| [In-game editors](../features/editors.md) | Channels, tags and announcements changed in a window |
| [A network](../features/network.md) | Every server and the proxy as one chat |
| [Discord](../features/discord.md) | Channels bridged to Discord, events and staff actions posted, `/online` |

Every window is a file, every command can be renamed or turned off, and every line a player reads is in a language
file you can add to. Seven languages ship.

## Read next

- [Installation](installation.md): the jar, the files, checking the install.
- [Concepts](concepts.md): modules, channels, formats, the database and language keys.
- [Your first chat](first-chat.md): shape the chat of a new server in ten minutes.

Related: [Commands](../commands/overview.md), [Permissions](../permissions/reference.md)
