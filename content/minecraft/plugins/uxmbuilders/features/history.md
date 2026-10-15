---
title: Build history
order: 211
description: Every finished and cancelled build, with what was paid and refunded.
icon: history
---

`/builders history`, or **Build History** in the main menu, lists a player's completed and cancelled
builds, newest first, 21 to a page.

## An entry

| Field | Completed | Cancelled |
|---|---|---|
| Package and job id | yes | yes |
| When it finished | yes | yes |
| Blocks | the total | placed out of total, with the percentage |
| Paid | yes | yes |
| Refund | | yes |
| Reason | | owner, staff, land lost, ran out of materials, not resumed after a restart... |
| Where | world and coordinates | world and coordinates |

A refund the economy refused shows as `0`, which is what the player actually received.

## Teleporting

With `uxmbuilders.history.teleport`, clicking a completed build teleports to the top of it. Teleports are
limited to one per `history.teleportCooldown` seconds, which `uxmbuilders.bypass.cooldown` skips.

## Other players

`/builders history <player>` needs `uxmbuilders.history.others`. Any player who has played on the server
can be looked up, online or not.

## Settings

| Key | Default | Meaning |
|---|---|---|
| `history.teleportCooldown` | `30` | Seconds between teleports |

The layout, the 21 entry slots and every line of text are in `menu/history.yml`.

## Notes

- The history reads the job table, so builds from before the update that added it appear too, without
  a refund amount or a reason.
- Both permissions default to operators.

Related: [Restarts and refunds](recovery.md), [Commands](../commands.md)
