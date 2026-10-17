---
title: Events and runs
order: 32
description: Starting, pausing, ending, changing and deleting an event.
icon: calendar-clock
---

An event is started, played and ended as a run. Only one event runs at a time, and everything below
can be done from the menus or with commands.

## Starting

| Way | How |
|---|---|
| Command | `/sr start [id]` |
| Manage menu | **Start Event** |
| Event list | Right-click the event |
| Schedule | A weekly slot in [auto-start.yml](auto-start.md) |

An event needs at least one item before it can start. Starting a draft or a completed event begins
a new run with empty scores; starting a paused event resumes it. While another event is running or
paused, the start is refused: finish or pause that one first.

A start sends the event's `start-broadcast`, and a resume its `resume-broadcast`. Both are written
in the [event file](../config/event-files.md).

## Pausing

`/sr pause`, a right-click in the event list, or **Pause** in the stop question pauses the running
event. Scores and the remaining time are kept, deposits are refused, and starting it again picks up
where it stopped.

## Ending

A run ends by itself when its time is up. Players are warned before that, at each time listed in
`end-warnings` (one hour and ten minutes before, by default).

To end it early, type `/sr stop` (or `/sr complete`, `/sr finish`). Chat asks what to do, with
clickable buttons that work once, for 60 seconds:

| Button | Result |
|---|---|
| Finish + Pay | Ends the run now and pays the rewards |
| Stop Unpaid | Ends the run now and pays nothing |
| Pause | Pauses the run instead |
| Cancel | Leaves it running |

When a run ends, the event file's `complete-broadcast` announces the results. A run stopped unpaid
sends `unpaid-broadcast` instead, and a run with nobody to reward sends `no-reward-broadcast`. A run
is never paid twice.

## Changing an event

| Change | How | Effect on a running event |
|---|---|---|
| Name | **Rename Event** | Immediate |
| Length | **Set Duration**, or `/sr duration <time> [id]` | The current run's length changes too |
| Items and points | The manage menu | Immediate |
| Rewards and announcements | The [event file](../config/event-files.md), then `/sr reload` | Immediate |

A `duration` changed in the event file instead applies from the next run; the current run keeps its
end time.

Removing an item from a running event raises a question for the points already earned with it.
`freeze-removed-item-scores: true` (the default) keeps them; `false` takes them away. Set it in
[general-settings.yml](../config/general-settings.md).

## Deleting

`/sr delete [id]` deletes an event after asking for confirmation. A running or paused event must be
finished first. Its finished runs stay in the history.

Related: [Depositing](depositing.md), [Rewards](rewards.md), [Commands](../commands.md)
