---
title: Permissions
order: 80
description: The player permission, one permission per admin command, and the two extras.
icon: shield-check
---

Players need one permission, which everyone has by default. Every admin command has its own, and
the admin buttons in the menus check the same permission as their command.

## Players

| Permission | Allows | Default |
|---|---|---|
| `uxmsupplyrush.play` | Opening `/sr`, depositing, and browsing the history | Everyone |

## Admins

`uxmsupplyrush.command.*` grants every permission below. All of them default to operators.

| Permission | Allows |
|---|---|
| `uxmsupplyrush.command.help` | The `/sr help` command list |
| `uxmsupplyrush.command.events` | `/sr events`, and the manage button of the main menu |
| `uxmsupplyrush.command.create` | The **Create New Event** button |
| `uxmsupplyrush.command.manage` | `/sr manage` and every button of the manage menu |
| `uxmsupplyrush.command.start` | `/sr start`, and chat warnings when a scheduled start would overlap a running event |
| `uxmsupplyrush.command.pause` | `/sr pause` |
| `uxmsupplyrush.command.stop` | `/sr stop` and its buttons |
| `uxmsupplyrush.command.duration` | `/sr duration` |
| `uxmsupplyrush.command.delete` | `/sr delete` |
| `uxmsupplyrush.command.logs` | `/sr logs`, the log menus, and any player's deposits in past leaderboards |
| `uxmsupplyrush.command.export` | `/sr export`, and the export button of run summaries |
| `uxmsupplyrush.command.reload` | `/sr reload` |
| `uxmsupplyrush.command.management` | `/sr management`, and moving the management server |

## Extras

| Permission | Allows | Default |
|---|---|---|
| `uxmsupplyrush.bypass.creative-deposit` | Depositing in creative mode while `creative-deposit` is `DENY` | Operators |
| `uxmsupplyrush.seedebugs` | Seeing debug output in chat while `debug` is on | Operators |

## A staff group

For staff who run tournaments but should not create or delete them:

```
uxmsupplyrush.command.events
uxmsupplyrush.command.manage
uxmsupplyrush.command.start
uxmsupplyrush.command.pause
uxmsupplyrush.command.stop
uxmsupplyrush.command.logs
```

Related: [Commands](commands.md)
