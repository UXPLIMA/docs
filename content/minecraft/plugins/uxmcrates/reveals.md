---
title: "Reveals"
order: 50
description: "An animation file under `animations/` decides how a reward is shown and nothing else."
---

An animation file under `animations/` decides how a reward is shown and nothing else. The reward is decided and
stored before the first frame, so no animation can change what a player wins, and a skip or a closed window
still pays out. `opening.skippable` lets a shift click or a closed window end the reveal at once, after
`opening.skip-after-ms`.

## The six reveals

| `reveal` | Drawn in | A Bedrock player gets |
|---|---|---|
| `inventory-reel` | A window | The same. |
| `inventory-turn` | A window | The same. |
| `window` | A window the file draws | The same. |
| `world-orbit` | Packets, per viewer, around the crate block | The file's `bedrock-fallback`, a window reveal. |
| `world-pillar` | Packets, per viewer, rising from the crate block | The file's `bedrock-fallback`, a window reveal. |
| `immediate` | Nothing | The same. |

A world reveal sends display entities as packets: nothing is spawned or saved, and two players opening at once
never share one animation. A server with no packet layer, or an opening that is not at a placed block, plays the
window reveal instead. `bedrock-fallback` must name a window reveal; a world reveal named there is refused and
the reel is used.

## Common keys

`duration-ms`, `frame-ms`, `easing` (`linear`, `ease-in`, `ease-out`, `ease-in-out`, or any uxmLib curve such as
`sine-in-out`), `decoys`, `spread` and `rise` in blocks, `tick-sound`, `reveal-sound` and `bedrock-fallback`. A
world reveal may also carry a `path` with one expression per axis, `x`, `y` and `z`. The same file format is
used for a crate's `idle-effect`, which stops when nobody is near.

## A window the file draws

The reveal `window` reads a `window` block with `rows` (one to six), `decor` and `lanes`. `decor` is a list of
`{ material, slots }` panes that never move. `lanes` is a list of the parts that move, drawn in order, so a later
lane covers an earlier one. A slot is a number or a range such as `"10-16"`, which also runs backwards as
`"34-28"`.

A lane takes `slots`, a `mode`, and optionally `faces`, `win-slot`, `start-ms`, `end-ms` and `easing`:

| `mode` | What a step does |
|---|---|
| `scroll` | The faces travel one slot toward the lane's first slot. |
| `each` | Every slot draws a face of its own. |
| `same` | Every slot shows one face, drawn again. |
| `random` | One slot, picked at random, draws a new face. |

A lane with no `faces` shows the crate's rewards at their weights; with `faces` it shows those materials as
panes. Exactly one lane has a `win-slot`, and on its last step the reward is in that slot. The shipped `wheel`
file is a commented example. A block that does not read is named in the console and the reel plays instead, as
it does for an unknown reveal, mode, slot or curve.

`opening.window` in `opening.conf` sets the `fill` pane shown in every uncovered slot and the `pointer` pane
above and below the slot the reel and the turn stop on. `""` turns either off.
