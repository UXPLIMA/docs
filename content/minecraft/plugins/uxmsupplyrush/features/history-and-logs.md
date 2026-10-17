---
title: History and logs
order: 36
description: Past runs for players, summary files for admins, and the log of everything that happened.
icon: history
---

Every finished run is kept: its final leaderboard, what each player deposited, and the items the
event accepted at the time. Deleting an event does not delete its history.

## For players

The **History** button in `/sr` opens two views:

| View | Shows |
|---|---|
| Server History | Every past run and its final leaderboard |
| My History | The runs the player took part in: their rank, their points, and what they deposited |

A run opens on its summary, then the player's deposits, then the full leaderboard.

## For admins

| Tool | What it does |
|---|---|
| Archived leaderboards | Click any participant to see their deposits in that run. Needs `uxmsupplyrush.command.logs` |
| `/sr export [id] [run]` | Writes a readable summary file of the running event, or of one finished run, to `history/` |
| **Export** button | The same, from a run summary. Needs `uxmsupplyrush.command.export` |
| `/sr logs [id]` | Lists an event's runs, then every logged action of one run |

Summary files are named `event-<id>-summary.yml` for the running event and
`event-<id>-run-<number>-summary.yml` for a finished run.

## Logs

The log records what happened during a run: deposits, starts and pauses, items added, removed or
repriced, and every reward given, with who did it and on which server.

```yaml
# general-settings.yml
log-cleaner:
  enabled: true
  older-than-days: 30
  logs-to-clean: 5000
```

The log cleaner keeps the in-game logs short. On every start and every five minutes it deletes up
to `logs-to-clean` logs older than `older-than-days`, oldest first. It never deletes the logs of the
run that is going on now, nor deposits from the last seven days.

Cleaning only affects `/sr logs`. Every log line is also written to `history/event-<id>.log`, which
is never cleaned, and the history of finished runs is never touched.

Related: [Events and runs](events-and-runs.md), [Commands](../commands.md)
