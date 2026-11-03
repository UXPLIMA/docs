---
title: Animation files
order: 512
description: Every key of an animation file, world paths, and the window a file lays out itself.
icon: clapperboard
---

One file per reveal under `animations/`; the file name is what a crate's `animation` or `idle-effect` names.

## Common keys

| Key | Meaning |
|---|---|
| `reveal` | `inventory-reel`, `inventory-turn`, `window`, `world-orbit`, `world-pillar` or `immediate` |
| `duration-ms` | How long the reveal runs |
| `frame-ms` | Time between two frames |
| `easing` | `linear`, `ease-in`, `ease-out`, `ease-in-out`, or any uxmLib curve such as `sine-in-out` |
| `decoys` | Materials that scroll past. Empty draws the crate's rewards at their weights |
| `spread` | Blocks: how wide a world reveal spreads |
| `rise` | Blocks: how high it rises |
| `tick-sound` | Played each frame; `""` for none |
| `reveal-sound` | Played when the reward lands |
| `bedrock-fallback` | The window reveal a Bedrock player, or an opening not at a block, gets |

## A world path

A `world-orbit` or `world-pillar` reveal may carry one expression per axis:

```hocon
path {
  x = "cos(t * tau() * 1.5) * spread"
  y = "rise * t"
  z = "sin(t * tau() * 1.5) * spread"
}
```

`t` runs from 0 to 1 over the reveal; `spread` and `rise` are the file's own numbers. Nothing about a shape is in the
code: change a number and it is a different shape.

## A window the file draws

The reveal `window` reads a `window` block:

```hocon
window {
  rows = 5
  decor = [ { material = "LIME_STAINED_GLASS_PANE", slots = [4] } ]
  lanes = [
    { slots = ["0-3", "5-8", "36-44"], mode = "each", faces = ["RED_STAINED_GLASS_PANE", "YELLOW_STAINED_GLASS_PANE"], easing = "linear" }
    { slots = ["10-16", 25, "34-28", 19], mode = "scroll", win-slot = 13 }
  ]
}
```

| Key | Meaning |
|---|---|
| `rows` | One to six |
| `decor` | `{ material, slots }` panes that never move |
| `lanes` | The moving parts, drawn in order: a later lane covers an earlier one |

A slot is a number or a range such as `"10-16"`, which also runs backwards: `"34-28"`.

A lane takes `slots`, a `mode`, and optionally `faces`, `win-slot`, `start-ms`, `end-ms` and `easing`:

| `mode` | What a step does |
|---|---|
| `scroll` | The faces travel one slot toward the lane's first slot |
| `each` | Every slot draws a face of its own |
| `same` | Every slot shows one face, drawn again |
| `random` | One slot, picked at random, draws a new face |

A lane with no `faces` shows the crate's rewards at their weights; with `faces` it shows those materials as panes.
Exactly one lane has a `win-slot`, and on its last step the reward is in that slot. `wheel.conf` is the commented
example.

A block that does not read is named in the console and the reel plays instead, as it does for an unknown reveal, mode,
slot or curve.

Related: [Reveals](../features/reveals.md)
