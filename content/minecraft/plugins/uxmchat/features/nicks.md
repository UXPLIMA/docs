---
title: Nicknames
order: 410
description: The name a player goes by in chat, its rules, and finding who is behind one.
icon: id-card
---

Module: `nicks`. `/nick <nick>` sets the name a player goes by in chat: `<display>` in formats, `%uxmchat_nick%`.
`/nick off` drops it. Node `uxmchat.nick`, given to operators only: give it to the ranks that may.

| Rule | Key under `nick` | Default |
|---|---|---|
| What a nick may be made of | `allowed`, a regular expression | `[A-Za-z0-9_]+` |
| Shortest | `min-length` | `3` |
| Longest | `max-length` | `16` |
| Names nobody may take, compared without case | `blocked` | `admin`, `owner`, `staff`, `moderator`, `console`, `server` |

A nick that another player uses, or that is another player's name, is refused.

| Staff command | Node |
|---|---|
| `/nick player <player> <nick>` | `uxmchat.nick.others` |
| `/nick realname <nick>` | `uxmchat.nick.realname`: who online goes by that nick |

Related: [config.conf](../config/config-conf.md#private-messages-friends-mail-quick-replies-nicks)
