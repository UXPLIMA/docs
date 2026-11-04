---
title: Performance
order: 1150
description: What a protection check costs, how guard keeps it small, and how to measure it on your own server.
icon: gauge
---

A protection plugin runs on every block broken, every click, every step into a new chunk and every drop of
flowing water. Guard is built so that this cost stays small whatever the number of regions, and is nothing at
all for what no rule mentions.

## What a check costs

Measured with `./gradlew :core:guardBench` from the source, on the development server. Each row is a world with
that many regions spread over it; a check asks one action at one point.

| Regions | Compile, ms | Check at random points, ns | Check where players walk, ns | Asking every region, ns |
|---|---|---|---|---|
| 10 | 26 | 29 | 30 | 24 |
| 100 | 16 | 65 | 36 | 159 |
| 1,000 | 36 | 138 | 68 | 2,953 |
| 5,000 | 105 | 232 | 91 | 31,355 |
| 20,000 | 379 | 351 | 110 | 97,938 |

"Where players walk" checks points near each other, as a player's clicks are. "Asking every region" is what a
simple loop over all regions costs, for comparison. Your numbers will differ with the machine; their growth will
not.

On a live Folia server, ten players moving one block every tick through five template regions, with every
behaviour applied and taken back on each crossing, uxmEssentials took 1.7 percent of the busy time of the region
threads, at an average tick of 7.7 ms.

## How

- **Compiled, not looked up.** Every region's rules are flattened once, through its parents and template, into a
  table with one entry per action and role. A check reads one entry; it never walks a list of rules.
- **Per chunk.** A world is split into chunk columns of 16-block sections. Each section knows which regions can
  touch it; sections with the same regions share one cell. A section entirely inside its regions needs no shape
  test at all.
- **Only where needed.** A column is built the first time something happens in that chunk, and a chunk with no
  region keeps nothing in memory. An edit rebuilds only the columns near the edit.
- **Listeners only for rules that exist.** Guard registers its event listeners by family (building, interaction,
  damage, items, nature, explosions, hoppers) and only while some region has a rule in that family. The nature,
  explosion and hopper families also carry the [border test](concepts.md#parents-and-families), so they run once any
  world has a rule. For an event that moves something from one block to another, that is two region lookups (the
  families of the source and the target); for any other event whose action no rule names, one bit test. A server
  with no rules at all has no guard listener on any protection event.
- **No locks on the hot path.** Checks read an immutable snapshot that an edit replaces in one step, so any thread,
  on Paper or Folia, checks without waiting.
- **Little made per check.** The engine creates no objects while it decides; explanations (`/guard why`,
  inspect) are built only when asked for.

## Measuring your own server

- `/guard status` shows regions, chunk columns and region sets in memory, checks per second, and per action
  family the number of checks, refusals and the average nanoseconds. The first checks in each chunk include
  building its column, so read the averages after players have been around for a while.
- With [spark](https://spark.lucko.me/) installed, a profile shows guard's listeners under `uxmEssentials`.
- `worlds.<world>.high-frequency = false` turns off the busiest nature events for a world where those rules do
  not matter.
