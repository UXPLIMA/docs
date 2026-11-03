---
title: Event files
order: 503
description: The keys of an event file and the four shipped events.
icon: zap
---

`events/join.conf`, `first-join.conf`, `respawn.conf` and `world-change.conf`.

```hocon
enabled = true
delay = "0s"
actions = [
  "[message] @event.join.welcome"
]
```

| Key | Meaning |
|---|---|
| `enabled` | `false` and the moment hands out nothing |
| `delay` | How long to wait after the event; left out, at once |
| `actions` | Action lines run once, after the items are in place; the same syntax as an item's click list |

What each shipped event does is in [Triggers and events](../features/triggers-and-events.md#event-files).

Related: [Triggers and events](../features/triggers-and-events.md)
