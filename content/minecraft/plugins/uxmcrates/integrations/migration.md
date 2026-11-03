---
title: Migration
order: 905
description: Reading the crates of ExcellentCrates, CrazyCrates or SpecializedCrates into uxmCrates files, and the report.
icon: arrow-right-left
---

`/uxmcrates migrate <plugin>` (`uxmcrates.admin.migrate`) reads another plugin's crate set and writes uxmCrates files:
crates, keys, their rewards, and where the crates stand. ExcellentCrates keeps its placed blocks in each crate file and
CrazyCrates in `locations.yml`; both become the crate's `positions`.

| `<plugin>` | Reads |
|---|---|
| `excellentcrates` | `plugins/ExcellentCrates/` |
| `crazycrates` | `plugins/CrazyCrates/` |
| `specializedcrates` | `plugins/SpecializedCrates/` |

The source is read from its folder, so it does not have to be installed or enabled, and none of its code is loaded.
Without the folder the answer is `migrate.nothing`. A crate whose id already exists here is left alone and named in
the report.

## The report

The answer says how many crates and keys came across, and how many settings did not. A report in
`plugins/uxmCrates/migrations/` lists every value the reader saw and did not use, by path and value. Read it before
going live: a setting with no equivalent is named there, never dropped quietly.

## Steps

1. Copy the other plugin's folder into `plugins/` if it is not there.
2. Run `/uxmcrates migrate crazycrates`.
3. Read the report in `migrations/`.
4. Run `/uxmcrates reload` to load what was written, then `/uxmcrates doctor`.
5. Check that the crates stand where they did; place any that did not come across with `/uxmcrates place`.
6. Remove the old plugin.

Key balances held in the other plugin's database are not carried across; give them again with
`/uxmcrates key give`.

Related: [Operator commands](../commands/admin.md#the-plugin)
