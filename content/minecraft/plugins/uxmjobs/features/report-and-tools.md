---
title: Report and tools
order: 416
description: The payout report, income alerts, preview, inspect, prune and the explore table.
icon: clipboard-list
---

## The payout report

A plugin whose purpose is to create money should be able to say how much it created and from what.

```hocon
report {
  enabled              = true
  retention-days       = 7
  window-minutes       = 60
  alert-multiple       = 10.0
  alert-minimum-sample = 5
}
```

`/uxmjobs report` prints one line per job: the amount over the window and how many payments made it. `/uxmjobs report
menu` opens the [report window](../menus/report.md): one tile per job per currency, largest first, with its share of
the window, and a total. It reads a cache and never waits for the database.

## Income alerts

`/uxmjobs alerts` lists players who earned `alert-multiple` (10) times the median of their job over the window, once
at least `alert-minimum-sample` (5) players worked it. It is a heuristic and it will sometimes be wrong. `0` turns it
off.

## preview and inspect

`/uxmjobs preview <job>` prints every row at levels 1, 10, 50 and 100 and where the bound holds it, before anybody
plays. `/uxmjobs inspect` says what the block or creature you look at is worth in each job.

## prune

Rows that grow without bound carry a retention: the ledger (`ledger.retention-days`), decay counters
(`decay.retention-days`), payouts (`report.retention-days`), explored chunks (`explore.retention-days`) and ended
leaderboard windows (`leaderboard.keep-windows`). They are pruned on a timer; `/uxmjobs prune` does it now.

## flush

Decay counters, income windows and the payout queue reach the disk every `storage.flush-seconds` (30) and on
shutdown. The number decides how much a crash costs.

Related: [Admin commands](../commands/admin.md)
