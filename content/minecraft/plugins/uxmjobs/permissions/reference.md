---
title: Reference
order: 601
description: Every permission node of uxmJobs with its default and what it allows.
icon: list-checks
---

## Players

`/jobs` and `/contracts` need no node. A player's own `/uxmjobs lang`, `lang set` and `lang reset` need none either.

## Nodes you name

The plugin reads a number off these and never learns your group names. None is declared, so none is held until you give
it.

| Node | Gives |
|---|---|
| `uxmjobs.slots.<n>` | That many jobs at once; the highest wins |
| `uxmjobs.level-cap.<job>.<n>` | That job's level cap lifted to `n`; the highest wins (the prefix is `level-cap.node` in the job file) |
| `uxmjobs.tax.<n>` | A lower tax rate; see [Income cap and tax](../features/income-cap-and-tax.md#tax) |
| `uxmjobs.leave.discount` | The shipped jobs' cheaper leave cost |
| Any `leave-cost.nodes` key | Its leave cost; the cheapest held wins |
| Any booster `node` in `boosters.conf` | That booster |
| Any `permission` of a shop line | That line |
| `uxmjobs.perk.miner.deep`, `uxmjobs.perk.miner.debris` | Granted by the shipped miner rewards at 25 and 75, for your other plugins to read |

## Declared

| Node | Default | Allows |
|---|---|---|
| `uxmjobs.tax.bypass` | op | Being paid without tax |
| `uxmjobs.admin.*` | op | Everything an operator does |
| `uxmjobs.admin.version` | op | `/uxmjobs`, `version` |
| `uxmjobs.admin.doctor` | op | `doctor` |
| `uxmjobs.admin.reload` | op | `reload` |
| `uxmjobs.admin.language` | op | `lang set-player`, `lang reset-player` |
| `uxmjobs.admin.editor` | op | `editor` and its windows |
| `uxmjobs.admin.experience` | op | `experience` |
| `uxmjobs.admin.setlevel` | op | `setlevel` |
| `uxmjobs.admin.points` | op | `points` |
| `uxmjobs.admin.reset` | op | `reset` |
| `uxmjobs.admin.boost` | op | `boost start`, `boost stop` |
| `uxmjobs.admin.inspect` | op | `inspect` |
| `uxmjobs.admin.preview` | op | `preview` |
| `uxmjobs.admin.report` | op | `report`, `report menu`, `alerts` |
| `uxmjobs.admin.prune` | op | `prune` |
| `uxmjobs.admin.zone` | op | Every `zone` subcommand, and using the wand |
| `uxmjobs.admin.sign` | op | `sign update`, and hanging or taking down a job sign |

`uxmjobs.admin.*` declares no children, so it grants nothing through a permission plugin that does not read wildcards
itself; operators hold every node through their default. `uxmjobs.tax.bypass` defaults to op, so operators pay no tax
when it is on.

Related: [Commands](../commands/overview.md)
