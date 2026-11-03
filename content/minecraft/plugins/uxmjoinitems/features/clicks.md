---
title: Clicks
order: 403
description: Left and right click actions, the two verbs of this plugin, the shared action lines and the old title form.
icon: mouse-pointer-click
---

`left-click` and `right-click` are lists of action lines, run in order, with no ceiling on the list.

## This plugin's verbs

| Verb | Does |
|---|---|
| `[server] <name>` | Moves the player through the proxy, on `network.proxy-channel` |
| `[menu] <command>` | Closes the open screen, then runs the command as the player |

## Action lines

The rest are uxmLib's, the grammar every UXM plugin reads: `[message]`, `[broadcast]`, `[actionbar]`, `[title]`,
`[subtitle]`, `[bossbar]`, `[particle]`, `[sound]`, `[effect]`, `[console]`, `[player]`, `[close]`, `[take-money]`,
`[give-money]` and `[take-item]`. Any of them may carry `{delay=, chance=, repeat=, every=, if=}` between the verb and
the payload:

```
[message] {delay=40, chance=0.5} <body>Sometimes, two seconds later.
```

`%player%` inside a command or a server name becomes the player's name; `<player>` inside a message or a title does the
same.

## The old title form

This plugin's own spelling of a title is kept and still works:

```
[title] <title>|<subtitle>|<fade-in>|<stay>|<fade-out>
```

It is read as `[subtitle] <title> | <subtitle> <in> <stay> <out>`, which takes its three times in seconds rather than
as durations. Write either.

## Holding the button

Holding the button down runs the actions once per [cooldown](cooldowns.md).

Related: [Item files](../config/item-files.md#clicks), [Triggers and events](triggers-and-events.md)
