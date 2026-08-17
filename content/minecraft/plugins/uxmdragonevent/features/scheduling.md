---
title: Scheduling
order: 32
description: The weekly timetable, its timezone, and telling players when the next one is.
icon: calendar-clock
---

Events run themselves on a weekly timetable.

```yaml
auto-start:
  enabled: true
  max-per-month: 100
  timezone: 'GMT+3'

  days:
    Sunday:
      times: "01:15, 12:05, 20:00"
    Monday:
      times: "01:15, 12:05, 20:00"
```

| Key | Meaning |
|---|---|
| `enabled` | Whether the schedule runs at all |
| `max-per-month` | A ceiling on automatic starts per month |
| `timezone` | The zone the times are written in |
| `days.<Day>.times` | Comma-separated 24-hour times |

Days are the English names (`Sunday` through `Saturday`) and a day you leave out simply has no
events. A day with an empty `times` is the same as leaving it out.

## Timezone

`timezone` is the zone the times mean, not the server's zone. Written as `GMT+3`, or as a zone id
like `Europe/Istanbul`.

Set this to the timezone your **players** are in. A schedule written in UTC on a server whose
community is all in one country produces events at times nobody is awake for, and daylight saving
makes a fixed GMT offset drift by an hour twice a year: a named zone handles that, an offset does
not.

## max-per-month

A hard ceiling on automatic starts. With three events a day it takes about 93 in a month, so the
default of 100 allows the full schedule with a little room.

It is a safety net, not a schedule: use it to stop a misconfigured timetable from running events
continuously, and set it comfortably above what your `days` actually ask for.

Manual `/dragon start` is not counted against it.

## What players see

Two placeholders make the schedule visible:

```
%dragonevent_remaining%   time until the next event
%dragonevent_next%        when the next event is
%dragonevent_last%        when the last one was
```

Formatted with:

```yaml
settings:
  remaining-time-format: '%days% days %hours% hours %minutes% minutes %seconds% seconds'
  next-event-time-format: 'dd/MM HH:mm'
```

`remaining-time-format` uses `%days%`, `%hours%`, `%minutes%` and `%seconds%`.
`next-event-time-format` is a Java `SimpleDateFormat` pattern.

Put `%dragonevent_remaining%` on a scoreboard or a hologram at spawn: the schedule only works as a
draw if players know when the next one is.

## Announcing ahead of time

Discord notifications go out before an event starts:

```yaml
notify-times:
  - 30
  - 15
  - 5
  - 1
```

Minutes before the start. See [Discord announcements](discord.md).

## Event length

```yaml
settings:
  dragon-event-time: 15
```

Minutes. When the clock runs out with the dragon still alive, the event ends with `timed-out`, no
winner, no rewards.

Fifteen minutes with two rebirths is a sensible starting point. Check the first few real events: if
the dragon dies with ten minutes to spare, raise its health rather than shortening the timer, and if
it times out with players still fighting, the health is too high for your attendance.

## Disabling automatic starts

`auto-start.enabled: false` leaves `/dragon start` as the only way to run one, which is the right
setting while you are still tuning the fight.
