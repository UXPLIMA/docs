---
title: Overview
order: 301
description: The two commands, their aliases, and how a missing permission is answered.
icon: terminal
---

| Command | Aliases | For |
|---|---|---|
| [`/quests`](quests.md) | `/quest`, `/q` | Players |
| [`/uxmquests`](admin.md) | `/uxmq`, `/questsadmin` | Operators |

Every name and alias is in [commands.conf](../config/commands-conf.md), which can rename, alias or switch off any
command and branch; a change there needs a restart. Two branches ship with an extra word: `reroll` also answers to
`swap`, `lang` to `language`.

A player branch's node is held by everybody, so the branch stays visible and a player without it is refused with
`command.no-permission`. An operator branch is hidden from anybody who lacks its node.

`<group>` and `<language>` and `<player>` are suggested as you type. A `<time>` is a span such as `30m`, `12h` or `7d`.

Related: [Permissions](../permissions/reference.md)
