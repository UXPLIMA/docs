---
title: protection.conf
order: 639
description: Rules that keep players from losing what they cannot get back.
icon: shield
---

Rules that keep a player from losing what they cannot get back.

The file is `plugins/uxmSkyblock/modules/protection.conf`. Every default below is the shipped value.

## `obsidian-recovery`

Lava that met water and turned to obsidian can be scooped back up with an empty bucket.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` |  |
| `accidental-only` | `true` | true: only obsidian that formed from lava and water can be scooped, so obsidian a player placed or mined cannot be turned into lava. false: any obsidian on an island can. |
| `expiration-seconds` | `60` | How long, in seconds, obsidian that formed stays scoopable. |

## `void-recovery`

A player who falls into the void is brought back to the island they fell from instead of dying.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` |  |
| `members` | `true` | Whether the island's own members are saved, and whether visitors are. |
| `visitors` | `true` |  |
| `threshold-y` | `-64` | The height below which a falling player counts as lost. The world's floor on Paper 26 is -64. |
| `fall-damage-shield-seconds` | `10` | Seconds after the rescue in which the player takes no fall damage. |
| `pvp-shield-seconds` | `10` | Seconds after the rescue in which no other player can hurt them. 0 for none. |

## `kinetic-ward`

Where a player lands from a teleport or an ender pearl, hostile mobs close by are pushed away, so nobody arrives inside a crowd of them. Nothing is killed or despawned.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` |  |
| `radius` | `5.0` | How far, in blocks, from the landing spot mobs are pushed. |
| `repulsion-force` | `1.5` | How hard they are pushed away, and how far up. Higher values throw them further. |
| `vertical-lift` | `0.35` |  |
