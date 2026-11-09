---
title: Restarts and refunds
order: 308
description: What happens to a build when the server stops, and what each kind of cancel pays back.
icon: life-buoy
---

Progress is checkpointed every `database.saveIntervalSeconds` and on every state change. After a
restart, unfinished builds come back **paused**, and their owner decides what happens next: the reason
for the restart is unknowable, so nothing resumes on its own.

Builds are brought back on the first tick after startup, once every plugin has enabled, so an economy
plugin that loads after uxmBuilders is there for any refund. A server that
[shares its database](../config/database.md#several-servers-on-one-database) brings back only the builds
it started itself.

## After a restart

When the owner is online, or as soon as they join, the chat offers two clickable choices:

| Choice | Result |
|---|---|
| **Resume** | The build carries on from where it stopped |
| **Cancel + refund** | The build ends and pays back the unbuilt share of the price, with no penalty |

Blocks the world lost in the crash (the database saves more often than the world) are put back first.
Progress, and with it any refund, stays where it was: breaking an early block before a restart cannot
turn a finished house into a near-full refund.

A build whose world is not loaded yet stays in the database, without a refund, and comes back as soon
as that world loads.

## Refunds by reason

| Reason | Refund |
|---|---|
| Owner cancels | `economy.refund.onCancel`: see below |
| Staff cancels | The full price |
| Internal error | The full price |
| Land lost | The unbuilt share, no penalty |
| Not resumed after a restart | The unbuilt share, no penalty |
| Materials never arrived | `onCancel`, as for an owner cancel |

`onCancel` is `FULL`, `NONE` or `PARTIAL`. Partial pays the unbuilt share, capped at
`economy.refund.maxPercent` of the price:

```
refund = price x min(1 - progress, maxPercent / 100)
```

A 1,000 build cancelled at 30% with the default 80 cap refunds 700. The same build cancelled at 10%
refunds 800, not 900.

The job menu's cancel button shows the exact amount before you shift-click it.

## When a refund is paid

A cancelled build is written to the database as cancelled **before** any money moves. A crash between
the two can therefore never pay a refund twice. If the database refuses that write, nothing is paid,
the owner is told the refund failed, and the build comes back, paused, after the next restart. A
finished build is likewise stored as completed before it counts toward the owner's totals.

## When a refund cannot be paid

If the economy plugin refuses the deposit, or was removed after the build was paid for, the player is
told the refund failed and to contact staff, the audit log records `REFUND_FAILED`, and the build
history shows no refund. Nothing claims money that was never paid.

## Other pauses

| Pause reason | Ends when |
|---|---|
| Owner offline | The owner joins again |
| World not loaded | The world loads; if the owner is offline, when they join |
| Out of materials | The warehouse holds the item |
| Paused by hand | Someone resumes it |

## Notes

- `database.crashRecovery: false` refunds the unbuilt share of every unfinished build at startup instead
  of asking. A build whose package no longer exists is refunded the same way.
- A build whose schematic file changed since it started carries on when the new file gives the same
  order of blocks. A different shape closes it and refunds the unbuilt share, because its saved
  progress refers to the old file.
- A blueprint whose request was still being processed when its holder logged off, or when the server
  stopped, is handed back on their next join, even after a restart.

Related: [Warehouse](warehouse.md), [Build history](history.md)
