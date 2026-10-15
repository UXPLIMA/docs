---
title: Skyblock
order: 10
description: The classic mode, a small island in the void, and the four kinds of it the plugin
  ships.
icon: cloud
---

A Skyblock island is a platform in the void, pasted from a schematic. It is the mode every other
one builds on: the bank, upgrades, missions, levels, warps, the vault and every other feature
work the same on every mode's islands.

## The kinds it ships

| Preset | The island |
| --- | --- |
| `classic` | The traditional skyblock start, with an oak tree and the essentials |
| `desert` | An arid island of cacti, dead bushes and sand |
| `nether` | A volcanic island of crimson growth and quartz veins |
| `cave` | A cavern island of stone, dripstone and ore |

Each is a block in `modules/presets.conf` with `mode = skyblock`, or no `mode` at all, since an
unknown or missing mode is skyblock. Add a block and a new kind appears in the island window and
in `/is create`, with no line of Java.

## The platform

`schematic` names the Sponge schematic the island is pasted from, a path under the plugin's data
folder such as `schematics/classic.schem`. The plugin writes the platform it ships to that path on
its first start, so the file is there to change.

- Change it with WorldEdit, FAWE, or `/is schematic save`. None of them needs to be installed for
  the island to paste.
- Without the file the shipped platform is laid.
- The centre of the schematic is the block players arrive standing on.

`presets.paste` in the same file says how a schematic is put down. A paste works each chunk on the
thread that owns it, `blocks-per-tick` positions a tick and `chunks-at-once` chunks at a time, so a
large island spreads over ticks rather than stopping one. `paste-air`, `entities` and `biomes` say
whether the schematic's air, entities and biomes are carried.

## A preset's settings

| Setting | What it does |
| --- | --- |
| `display-name`, `description` | The kind's name and description, a `@key` read in each player's language |
| `icon` | The item its tile wears in `menus/island-create.conf` |
| `order` | Where its tile stands, the lowest first |
| `permission` | A node a player needs to start it. Left out, everyone may |
| `biome` | The island's biome, falling back to `PLAINS` |
| `schematic` | The platform it pastes |
| `start` | What builds the island, in order. Left out, `["uxm:platform"]` |
| `world` | The world its islands are made in. Left out, the island world of `config.conf` |
| `dimensions` | How it starts in the Nether, the End and any other dimension |

## Commands

| Command | What it does |
| --- | --- |
| `/is create [preset]` | Creates your island, from the window or the kind written |
| `/is home` | Takes you to your island |
| `/is nether`, `/is end` | Take you to your island in the Nether and the End |
| `/is values` | Shows what each block adds to an island: its worth, its points and the levels they make |
| `/is level` | Shows the island level and block worth |

`/is values` opens `menus/block-values.conf`, the most points first. A Bedrock player gets it as a
form.

Every word is yours to rename, alias or turn off in `commands.conf`.
