---
title: Automatic starts
order: 35
description: A weekly schedule in auto-start.yml that starts events without anyone online.
icon: alarm-clock
---

`auto-start.yml` starts events at set times every week. Each start runs the event for its own
length, so the schedule decides when an event begins and the event decides when it ends.

```yaml
enabled: true
timezone: "GMT+3"
max-per-month: 0
event-key: "weekly-rush"
days:
  friday:
    - time: "18:00"
  saturday:
    - time: "12:00"
      event-key: "diamond-week"
    - time: "21:00"
```

| Key | What it does |
|---|---|
| `enabled` | Turns the schedule on |
| `timezone` | The time zone the times are read in, for example `GMT+3` or `Europe/Istanbul`. A name that is not recognised falls back to `GMT` |
| `max-per-month` | The most automatic starts in a calendar month. `0` means no limit |
| `event-key` | The event a slot starts when it does not name its own |
| `days` | `monday` to `sunday`, each with a list of times (`HH:mm` or `HH:mm:ss`) |

A slot can name its own `event-key`, as `diamond-week` does above. The key of an event is on the
info item of its manage menu.

## When a slot is skipped

A slot fires once a week and is skipped, with a warning in the console, when:

| Situation | |
|---|---|
| Another event is running or paused | Events never overlap |
| The monthly limit is reached | Counted per calendar month |
| The event does not exist or has no items | Fix the key or add items |

A slot missed while the server was off is not made up later. When a scheduled start would overlap
the event that is running now, admins with `uxmsupplyrush.command.start` are warned in chat ahead
of time.

The `%supplyrush_next_start%` [placeholder](../placeholders.md) shows when the next slot fires. On a
network, edit this file on the management server and run `/sr reload` there; see
[Multiple servers](multi-server.md).

Related: [Events and runs](events-and-runs.md)
