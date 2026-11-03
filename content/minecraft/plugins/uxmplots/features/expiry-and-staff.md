---
title: Expiry and staff
order: 414
description: Inactive plots, the report queue, the staff bench and the staff commands.
icon: user-cog
---

## Expiry

```hocon
expiry {
  warn-after-days       = 30
  tell-every-hours      = 24
  expire-after-days     = 90
  release-automatically = false
}
```

Worked out from when the owner was last seen, every time anybody asks. After `warn-after-days` the owner is told at
login and their members every `tell-every-hours`. After `expire-after-days` the plot is marked and appears in
`/uxmplots inactive`; with `release-automatically` it is taken back instead, a picture always taken first. `0` in
`expire-after-days` turns it all off.

## The report queue

`/uxmplots reports` opens the [queue](../menus/staff.md). Choose a report, then `valid`, `cleaned` or `unfounded`; or
`/uxmplots report <decision> <plot>`.

## The staff bench

`/uxmplots staff` opens the [bench](../menus/staff.md#bench) over the plot you stand on: reset, snapshot, delete,
unlock and redstone, each shown only to holders of its node.

## Staff commands

`/uxmplots owner <player>` hands a plot over, `delete` takes it back with the blocks staying, `reset` and `snapshot`
work on any plot, `unlock` lets go of a contest lock. See [Admin commands](../commands/admin.md).

Related: [Community](community.md#reports)
