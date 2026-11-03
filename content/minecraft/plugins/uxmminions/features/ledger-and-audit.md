---
title: Ledger and audit
order: 412
description: What minions produced and paid, and who moved a minion.
icon: notebook
---

## The ledger

```hocon
ledger {
  enabled        = true
  retention-days = 30
  report-lines   = 10
}
```

What each minion produced and what each seller paid, per minion and per owner: the number to look at when the economy
breaks. `/minions report` and `/uxmminions ledger` read it, up to `report-lines` types. Zero retention keeps every row.
`%uxmminions_earned%` totals it for a player.

## The audit

```hocon
audit {
  retention-days = 0
}
```

A row on every placement, break, pickup, upgrade, restore, removal and refused duplicate: the line you want when a
player says a minion vanished. Zero keeps everything, which is what ships.

Related: [Admin commands](../commands/admin.md)
