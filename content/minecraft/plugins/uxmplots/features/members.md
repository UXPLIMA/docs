---
title: Members and visitors
order: 303
description: The four grades of trust, bans, visit policies and shared building sessions.
icon: users
---

## Grades

| Grade | Lets a member |
|---|---|
| `build` | Place and break blocks |
| `interact` | Use doors, buttons, levers and the like |
| `container` | Open chests and other containers |
| `settings` | Change the plot's settings |

`/plot trust <player>` gives `build` and `interact`; `/plot trust <player> build,container` names them. The
[member window](../menus/panel.md#members) shows them as four switches. `/plot untrust` takes them away.

## Bans

`/plot ban <player>` keeps somebody off and takes their grades; `/plot unban` lets them back.

## Visiting

| Policy | Who may come |
|---|---|
| `open` | Anybody |
| `trusted` | The owner and members |
| `closed` | The owner alone |

A new plot starts with its world's `visit-policy`; `/plot visiting <policy>` changes it. This is not hiding: a closed
plot is still in the gallery with its rating and visits. `/plot hide` is the other decision.

## Sessions

```hocon
sessions {
  longest-minutes = 120
  most-guests     = 8
}
```

`/plot session open <minutes>` opens a plot of yours for a while; others `/plot session join` it on the plot or from
the [sessions window](../menus/community.md#sessions). A session grants building and nothing else: no chests, no
settings. Nothing about it is written down: it ends by itself, so the member list does not fill with people trusted
once for an afternoon.

Related: [Commands](../commands/plot.md#members-and-visitors)
