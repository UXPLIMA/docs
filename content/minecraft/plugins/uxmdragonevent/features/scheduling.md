---
title: Scheduling
order: 32
description: The weekly timetable, its timezone, the monthly cap, and telling players when the next event is.
icon: calendar-clock
---

Events can run themselves on a weekly timetable.

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
| `enabled` | Whether the timetable runs at all |
| `max-per-month` | The most events the timetable starts in one calendar month |
| `timezone` | The timezone the times are written in |
| `days.<Day>.times` | Start times for that day, separated by commas |

Days are the English names, `Sunday` to `Saturday`. A day you leave out, or leave empty, has no
events. Times are on the 24-hour clock with two digits each: `09:00`, not `9:00`. A time the plugin
cannot read is reported in the console, and the times after it on that day are skipped.

Changes take effect with `/dragon reload`.

## Timezone

`timezone` is the zone the times are written in, not the server's. It takes a region such as
`Europe/Istanbul` or an offset such as `GMT+3`.

Write it in your **players'** timezone, and prefer a region over an offset: a region follows daylight
saving time, while a fixed offset puts every event an hour off for half the year. A name the plugin
does not recognise counts as GMT, which shows up as every event starting hours early or late.

## When a scheduled event does not start

| Situation | What happens |
|---|---|
| An event is already running at that time | That start is skipped |
| The server is off at that time | That start is skipped. Missed starts are not made up later |
| `max-per-month` is reached | No more scheduled events until the first day of the next month |
| No lobby spawn is set | Staff online are told `spawn-not-set`, and the start is skipped |

## max-per-month

A ceiling on scheduled starts in a calendar month, counted in `timezone`. Three events a day is about
93 a month, so the default of 100 lets the full timetable run with a little room.

Treat it as a safety net rather than part of the timetable: set it comfortably above what `days`
asks for. Events started with `/dragon start` are not counted.

## What players see

```
%dragonevent_remaining%   time until the next scheduled event
%dragonevent_next%        when it starts
%dragonevent_last%        when the last scheduled event started
```

Formatted with:

```yaml
settings:
  remaining-time-format: '%days% days %hours% hours %minutes% minutes %seconds% seconds'
  next-event-time-format: 'dd/MM HH:mm'
```

`remaining-time-format` uses `%days%`, `%hours%`, `%minutes%` and `%seconds%`.
`next-event-time-format` is a Java date pattern: `dd/MM HH:mm` gives `28/10 20:00`.

Put `%dragonevent_remaining%` on a scoreboard or a hologram at spawn: a timetable only draws players
if they know when the next event is.

## Announcing ahead of time

With [Discord announcements](discord.md) on, the bot posts a countdown before each scheduled event,
at the minutes listed in `notify-times` in `discord.yml`. Events started with `/dragon start` get no
countdown, only the start message.

## Event length

```yaml
settings:
  dragon-event-time: 15
```

Minutes. When the time runs out with the dragon still alive, the event ends with `timed-out`: no
rewards, and everyone in it gets a loss.

Fifteen minutes with two rebirths is a sensible start. Watch the first few real events: if the
dragon dies with ten minutes to spare, raise its health rather than shortening the timer, and if
players are still fighting when time runs out, the health is too high for your attendance. See
[The dragon](the-dragon.md#health).

## Running without a timetable

`auto-start.enabled: false` leaves `/dragon start` as the only way to run an event, which is the
right setting while you are still tuning the fight. The schedule placeholders are empty while it is
off.
