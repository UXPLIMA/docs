---
title: Audit Log
order: 1351
description: A record of who did what to lands, towns and nations, read back with /uxm audit.
icon: scroll-text
---

The audit-log module writes a line for every finished change that uxmClaims reports: lands, members, roles,
flags, areas, warps, banks, towns, nations, relations, taxes and ruins. Staff read it back per land, town,
nation or player with `/uxm audit`.

Module `audit-log` · disabled by default · `modules/audit-log/config.yml`

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/uxm audit <target> [page]` | Shows the newest lines about a land, town or nation, or by or about a player. | `uxmclaims.admin.audit-log` |

A target is a name, tried as a land, then a town, a nation and a player. Put `land:`, `town:`, `nation:` or
`player:` in front to pick one, for example `/uxm audit town:Avalon 2`. Names may contain spaces; a number at
the end is the page. Lines still waiting to be written are written first, so an action shows up at once.

Each line shows the time, who did it (a player, or the kind of actor for automatic changes), the action, the
land, town or nation it happened to, the player it was about, and the rest of the event's details. The action is
the event name in lower case with hyphens: `LandMemberAddEvent` is `land-member-add`.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.admin.audit-log` | op | `/uxm audit` |

## Settings

| Key | Default | What it does |
|---|---|---|
| `keep-days` | `90` | How many days lines are kept. Older ones are deleted every 6 hours. `0` keeps them forever. |
| `flush-interval` | `5s` | How often waiting lines are written to the database in one batch. |
| `max-queued` | `20000` | How many lines may wait for the next write. When the database cannot keep up, the oldest waiting lines are dropped and the count is logged. |
| `page-size` | `10` | Lines per page of `/uxm audit`, at most 50. |
| `skip` | see below | Events not to record, by class name. |

The default `skip` list leaves out events that happen too often or repeat another line: `LandEnterEvent`,
`LandLeaveEvent`, `AreaEnterEvent`, `AreaLeaveEvent`, `WarpTeleportEvent`, `VaultOpenEvent`, `NewDayEvent`,
`UpkeepCalculateEvent`, and `BankDepositEvent`, `BankWithdrawEvent`, `BankTransferEvent` (which repeat
`BankTransactionEvent`).

## Notes

- Lines go to the table `uxmc_audit_log_entries`. They keep the names they were written with, so lines outlive
  deleted lands, towns and players.
- A war event is filed under both the attacker and the defender.
- Events that can still be cancelled are not recorded, only finished changes.
- On a [network](network.md) every server writes its own lines, and only the leading server deletes old ones.
- There is no `/claim log` or `/town log` menu for land owners yet, and no filter by action.

Related: [Automation](automation.md), [Webhooks](webhooks.md), [Network](network.md)
