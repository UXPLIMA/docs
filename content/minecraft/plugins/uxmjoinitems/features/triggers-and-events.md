---
title: Triggers and events
order: 402
description: The four moments that hand items out, their event files, delays and actions.
icon: zap
---

## Triggers

An item's `triggers` list names the moments that hand it out. An empty list means only `/uxmjoinitems give` and
`giveall` hand it out.

| Trigger | When |
|---|---|
| `join` | Every join, a first one included |
| `first-join` | A player's first join only |
| `respawn` | After a respawn |
| `world-change` | After a move to another world |

## Event files

Each moment is a file under `events/`:

| Key | Meaning |
|---|---|
| `enabled` | `false` and the moment hands out nothing |
| `delay` | How long to wait after the event; units `ms`, `s`, `m`, `h`, `d` |
| `actions` | Action lines run once, after the items are in place |

| Event | Shipped |
|---|---|
| `join.conf` | At once; `[message] @event.join.welcome`: "Welcome to the lobby, Name." |
| `first-join.conf` | After 1s; a title "Welcome" with "Right click the compass to start". It runs beside `join.conf`, not instead of it |
| `respawn.conf` | After 50ms, because the inventory is not writable at the instant the respawn event fires; no actions |
| `world-change.conf` | At once; no actions |

<Callout type="warning" title="Worlds on separate servers">

On a network where each world is its own server, set `enabled = false` in `world-change.conf`: the join trigger already
covers it, and the two together hand the items out twice.

</Callout>

Related: [Event files](../config/event-files.md), [Clicks](clicks.md#action-lines)
