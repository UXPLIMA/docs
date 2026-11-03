---
title: Joining and leaving
order: 403
description: Slots, seats, requirements, join fees, dialogs, the leave cost, the archive and the rejoin cooldown.
icon: door-open
---

## Joining

`/jobs join <job>`, a right click on the job board, or a join sign. A join is refused, with its own line, when:

| Refusal | Message key |
|---|---|
| The job does not exist | `job.unknown` |
| The player already works it | `job.already-held` |
| Every slot is used | `job.no-slot` |
| Every seat of the job is taken | `job.no-seats` |
| They left it less than `rejoin-cooldown-seconds` ago | `job.cooling-down` |
| A `requirements` line fails | `job.not-qualified` |
| They cannot pay the `join-cost` | `job.join-fee-unpaid` |

A job with a fee asks in a dialog first when dialogs are on (Minecraft 1.21.6 and `display.dialogs.enabled`).

## Slots

```hocon
slots {
  default = 3
  node    = "uxmjobs.slots."
}
```

A player holding `uxmjobs.slots.5` holds five; the plugin reads the number and never learns the group's name. The nodes
are read every `permissions.refresh-seconds` (20). A server that wants every job at once writes a large default and a
leave cost of zero.

## Seats

`seats` in a job file caps how many players hold it, offline holders included. `%uxmjobs_seats_left_<job>%` counts
what is free.

## Leaving

`/jobs leave <job>` charges the leave cost and leaves. With dialogs on and `employment.confirm-before-leaving` true, a
dialog shows the cost and what the archive keeps first. `/jobs quit <job>` and the [leave window](../menus/quit.md)
show the number before anything is paid. A player who cannot pay is refused with `job.leave-unpaid`.

## The archive

Leaving archives the job's experience: you keep what you did and lose what you are paid. Nothing is deleted on a timer.
Rejoining restores `keep-on-return` (0.5) of it, compounded once per departure, so a player who leaves and rejoins to
dodge a cost loses more each time: half, then a quarter. `/jobs archive` lists what is kept.

```hocon
employment {
  rejoin-cooldown-seconds = 3600
  keep-on-return          = 0.5
  confirm-before-leaving  = true
}
```

Related: [Commands](../commands/player.md), [config.conf](../config/config-conf.md#slots-permissions-employment)
