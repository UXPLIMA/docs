---
title: Inbox
order: 1325
description: Notifications kept for players who were offline when their land, town or nation needed them.
icon: inbox
---

The inbox module keeps the notices other modules send, such as an alliance proposal, a bank withdrawal or an
unpaid upkeep, for players who were offline at the time. On join a player is told how many are new, and
`/claim inbox` lists them by category.

Module `inbox` · disabled by default · `modules/inbox/config.yml` · requires [Lands](lands.md)

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/claim inbox [category]` | Opens your notifications. `category` is `all`, `personal`, `land`, `town` or `nation`. | `uxmclaims.command.inbox.use` |
| `/claim inbox readall` | Marks every notification as read. | `uxmclaims.command.inbox.use` |
| `/claim inbox clear` | Deletes your read notifications. | `uxmclaims.command.inbox.use` |
| `/town inbox` | Opens your notifications on the town tab. Needs [Towns](towns.md). | `uxmclaims.command.inbox.use` |

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.command.inbox.use` | everyone | `/claim inbox` and `/town inbox`. |

## Settings

| Key | Default | What it does |
|---|---|---|
| `keep-days` | `30` | Days a notification is kept. Older ones are deleted, read or not. `0` keeps them until `max-per-player` pushes them out. |
| `max-per-player` | `100` | Most notifications kept per player. The oldest go first. |
| `store-online` | `false` | Also keep notifications shown to online players, as read, so the inbox is a full history. With `false`, online players see them in chat only. |
| `join-reminder.enabled` | `true` | Send the `<count> new notifications` line on join. |
| `join-reminder.delay-seconds` | `2` | Seconds after joining before the line is sent. |

## Notes

- Which notices arrive here is decided by the sending module: [Relations](relations.md), [Bank](bank.md),
  [Taxes](taxes.md), [War](war.md) and [Ruins](ruins.md) use it when it is on. Without the inbox they only
  reach players who are online.
- Each notice's text is in this module's `messages.yml` under `notifications` (see [Languages](../configuration/languages.md)). A key missing
  there shows as the plain `custom` line.

Related: [Notifications](notifications.md), [Relations](relations.md), [Bank](bank.md), [Taxes](taxes.md)
