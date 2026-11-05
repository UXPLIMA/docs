---
title: Overview
order: 301
description: Presets, how a mode is chosen, and the modes side by side.
icon: layout-grid
---

A player chooses a preset in the [start window](../menus/island.md#start-an-island) or with `/is create <preset>`. A
preset with a `mode` starts an island of that game mode; one without is classic skyblock. The mode stays with the
island until it is reset.

## Which modes a server offers

`presets.modes` in [presets.conf](../config/presets-conf.md) names the modes offered. As shipped it is `["skyblock"]`:
only the four classic presets. A OneBlock server writes `["oneblock"]`, one that plays several writes each, such as
`["skyblock", "oneblock", "chunkblock"]`, and `["all"]` offers every mode. When one preset is left to offer, `/is`
makes it at once with no window. A mode taken out of the list still runs for the islands that play it already.

## The shipped presets

| Preset | Mode | What the player starts with |
|---|---|---|
| `classic` | | The traditional skyblock start, with an oak tree and the essentials |
| `desert` | | An arid island of cacti, dead bushes and sand |
| `nether` | | A volcanic island of crimson growth and quartz veins |
| `cave` | | A cavern island of stone, dripstone and ore |
| `oneblock` | OneBlock | One block in the void |
| `chunkblock` | ChunkBlock | A magic block in a single chunk |
| `acid_island` | AcidIsland | An island in an acid sea |
| `caveblock` | CaveBlock | An island deep in rock |
| `skygrid` | SkyGrid | A grid of lone blocks hanging in the sky |
| `boxed` | Boxed | A patch of real land in a box |
| `poseidon` | Poseidon | An island on the sea floor |
| `poseidon_ruin` | Poseidon | The sea floor with a shipwreck and a great ruin nearby |
| `poseidon_ruins` | Poseidon | The sea floor ringed by a shipwreck and old ruins |
| `poseidon_monument` | Poseidon | An island on a sunken temple of prismarine |
| `stranger_realms` | StrangerRealms | Real land with an Upside Down beneath it |
| `brix` | Brix | A creative plot on flat ground |
| `parkour` | Parkour | A course the team builds and anybody runs |
| `tradewinds` | TradeWinds | A trading vessel on the open sea |

A preset's name, description, icon, schematic, starting items and the permission a player needs to see it are in
[presets.conf](../config/presets-conf.md). Each mode also has its own switch, `enabled`, in its module file. Boxed and StrangerRealms need
a world of their own; see [Installation](../getting-started/installation.md#the-island-world).

## What is shared

Every mode keeps the island's team, roles, bank, upgrades, missions, warps, leaderboards and chat. A mode changes how
the island is laid out and what a player does on it. The words a mode adds are on
[Commands: Game modes](../commands/game-modes.md), and its staff verbs on [Staff](../commands/admin.md#isadmin).
