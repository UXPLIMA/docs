---
title: Poseidon
order: 80
description: An island on the sea floor where water is the air, with wrecks, ruins, a sunken
  temple, a lore book, a deep boss and growing coral.
icon: waves
---

A Poseidon island lies on the sea floor. Water is the air a Poseidon player breathes: in it they
see and breathe, out of it dry air and the sun burn them, and one who stops moving in the water
drowns. The Nether of a Poseidon island is flooded.

## Turning it on

1. Set `enabled = true` in `modules/poseidon.conf`.
2. Add `poseidon` to `presets.modes` in `modules/presets.conf`.

The plugin ships four kinds, each starting with `"uxm:platform"` and `"uxm:ocean"` and a wreck:

| Preset | What lies around it |
| --- | --- |
| `poseidon` | A shipwreck |
| `poseidon_ruin` | A shipwreck and a large ruin |
| `poseidon_ruins` | A shipwreck and a ring of small ruins |
| `poseidon_monument` | A shipwreck, with the island standing on a sunken temple |

## The ocean

The ocean fills the air around the island and never replaces a block the island already has.

| Setting | Shipped | What it does |
| --- | --- | --- |
| `ocean.above` | `24` | How many blocks of water stand over the arrival |
| `ocean.depth` | `8` | How many blocks of water lie under it, down to the floor |
| `ocean.radius` | `64` | How far the ocean reaches. Keep it inside half the grid spacing |
| `ocean.floor` | `SANDSTONE` | The block it stands on. A falling block is replaced by sandstone |

## Wrecks and ruins

Each entry of `wrecks` is a creation action a preset names as `"uxm:<name>"`:
`"uxm:shipwreck"`, `"uxm:ruin"` and `"uxm:ruins"` as shipped. Add an entry under a name of your own
and a preset can name it. A template only fills water and air, so it never breaks into the island.

| Key | What it does |
| --- | --- |
| `templates` | The structures to choose from, the game's own or a data pack's |
| `count` | How many are laid, spread evenly around the island |
| `distance` | How far from the centre each lies. Keep it inside the ocean's radius |
| `loot` | The loot tables the chests get, each in turn |

`monument` is the sunken temple `"uxm:monument"` lays right under the island: the island stands on
its roof, and a swimmer comes in by the door on each side. Its template `schematic:monument` is
`schematics/monument.schem`, written on the first start, to edit with any tool that writes a
Sponge schematic or with `/is schematic save`. Keep its height below the ocean's depth.

## The hazard

A player in creative or spectator is never touched.

| Setting | Shipped | What it does |
| --- | --- | --- |
| `hazard.check-every` | `1s` | How often each player on a Poseidon island is checked |
| `hazard.dry-damage` | `1.0` | Health taken at each check from a player out of the water |
| `hazard.sun-damage` | `2.0` | Health the sun adds, out of the water under an open daytime sky |
| `hazard.still-damage` | `2.0` | Health taken from a swimmer who has stayed still too long |
| `hazard.still-after` | `5s` | How long a swimmer may stay still before the water hurts |
| `hazard.still-reach` | `1.0` | How far a swimmer must move, in blocks, to count as moving |
| `hazard.rain-is-wet` | `true` | Whether rain keeps a player out of the water from drying |
| `hazard.water-effects` | water breathing, night vision | Effects the water gives. Keep each longer than `check-every` |

## The lore book

`/is lore` opens a book that follows the story: a book on Java, a form on Bedrock. After its
opening pages come the chapters of the missions `lore.chapters` lists, in order. A chapter is the
lines of `poseidon.lore.chapters.<mission>` in the reader's language file, shown once the reader
has finished that mission. The first chapter not yet earned is a page that names its mission, and
nothing after it shows. Leave the list empty for a book without a story.

## The deep boss

Now and then, while a player swims on a Poseidon island, something rises from the deep beside
them. One island sees at most one boss at a time.

| Setting | Shipped | What it does |
| --- | --- | --- |
| `boss.entity` | `elder_guardian` | The creature that rises |
| `boss.health` | `160` | Its health |
| `boss.check-every`, `boss.chance` | `1m`, `0.05` | How often a swimmer is looked at, and the chance each look |
| `boss.every` | `2h` | The least time between two bosses on one island |
| `boss.lasts` | `10m` | How long before it sinks back unslain |
| `boss.loot` | `deep_boss` | The table of `loot/` whoever slays it is given |

The effect lists `poseidon-boss-rises`, `poseidon-boss-slain` and `poseidon-boss-sinks` of
`modules/effects.conf` fire for every player on the island.

## Coral

Coral spreads slowly on a Poseidon island. At each look, `coral.samples` blocks within
`coral.radius` of each player are looked at, and a living coral block among them grows, by
`coral.chance`, a coral or a fan of its colour into the water above it, or a fan onto its side.
Plant coral blocks to grow a reef.

## Commands

| Command | What it does |
| --- | --- |
| `/is lore` | Reads the lore of the sea on a Poseidon island |
