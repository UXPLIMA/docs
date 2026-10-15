---
title: Welcome
order: 101
description: Buy a blueprint, place it, and watch an NPC crew put the structure up.
icon: door-open
---

uxmBuilders turns a WorldEdit schematic into something players buy and watch go up. A player buys a
**blueprint** in the shop, right-clicks the ground, adjusts a live preview, and confirms. A builder NPC
walks the site and lays the structure block by block, over minutes instead of one frame.

Requires **Paper 1.21.1 to 1.21.8, or 26.2**, and **WorldEdit**. Folia is not supported.

## The five ideas

| Idea | What it means |
|---|---|
| **Package** | A schematic on sale: a price, a category, an icon and rules |
| **Blueprint** | The item a purchase hands out; right-clicking it places the package |
| **Builder** | The crew that builds it: speed, material efficiency and an NPC look, upgraded by level |
| **Job** | One build in progress: it queues, builds, pauses, completes or is cancelled |
| **Warehouse** | An optional per-player material store that construction draws from |

## What that buys you

A structure that appears in one frame is a purchase. A structure a crew builds in front of the player
over five minutes is an event: neighbours come to watch, and the builder the player chose is visibly
faster than the one they started with. Builder levels are therefore something worth selling, and the
plugin keeps every speed under one server-wide ceiling so a fast builder never costs you TPS.

With the warehouse on, a build consumes real materials. A survival server can sell the convenience of
the construction while keeping the materials economy intact.

## Everything in the box

- Construction spread over many ticks under a shared time budget that shrinks when TPS drops
- Packet-based NPCs and previews: no server entities, no mob AI, no entity-count cost
- A live preview that turns red, and says why, where the build is not allowed
- Builders with levels, unlock prices, per-package speed affinity and a selectable NPC appearance
- Site preparation: obstacles cleared and a foundation laid under the floor before the first block
- Land protection through WorldGuard, GriefPrevention, Towny, PlotSquared and a generic fallback
- Placement rules for worlds, heights and distance from spawn
- Crash recovery that asks the owner instead of resuming blindly
- Trusted players, paid speed boosts, a build history and Discord notifications
- Eight languages, SQLite or MySQL storage, PlaceholderAPI and a developer API with events

## Where to go next

| You want | Page |
|---|---|
| Install it | [Installation](installation.md) |
| Learn the vocabulary | [Concepts](concepts.md) |
| Sell your first structure | [Your first build](first-build.md) |
| Understand the performance model | [Building and performance](../features/building.md) |
| Tune builders | [Builders](../features/builders.md) |
