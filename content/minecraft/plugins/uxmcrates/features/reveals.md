---
title: Reveals
order: 305
description: The six reveal kinds, the fourteen shipped animations, Bedrock fallbacks, and the reel's faithfulness.
icon: clapperboard
---

An animation file under `animations/` decides how a reward is shown and nothing else. A crate names one with
`animation = "<id>"`, and the same files serve as a crate's `idle-effect`.

## The six kinds

| `reveal` | Drawn in | A Bedrock player gets |
|---|---|---|
| `inventory-reel` | A window: faces scroll past a pointer | The same |
| `inventory-turn` | A window: faces turn over | The same |
| `window` | A window the file lays out itself | The same |
| `world-orbit` | Display entities around the crate block, per viewer, as packets | The file's `bedrock-fallback` |
| `world-pillar` | Display entities rising from the crate block, per viewer, as packets | The file's `bedrock-fallback` |
| `immediate` | Nothing: the reward is handed over at once | The same |

A world reveal spawns nothing and saves nothing, and two players opening at once never share one animation. A server
with no packet layer, or an opening that is not at a placed block (a `menu` crate, `/crates open`), plays the window
reveal named by `bedrock-fallback` instead. `bedrock-fallback` must name a window reveal; a world reveal there is
refused and the reel is used.

## The shipped animations

| File | Kind | Length | Frame | Easing | Fallback |
|---|---|---|---|---|---|
| `reel` | `inventory-reel` | 3000 ms | 100 ms | ease-out | `inventory-reel` |
| `turn` | `inventory-turn` | 2400 ms | 300 ms | linear | `inventory-turn` |
| `wheel` | `window` | 5000 ms | 100 ms | ease-out | `window` |
| `curtain` | `window` | 4000 ms | 200 ms | ease-in | `window` |
| `flicker` | `window` | 3500 ms | 150 ms | ease-out | `window` |
| `orbit` | `world-orbit` | 3200 ms | 160 ms | linear | `inventory-reel` |
| `crown` | `world-orbit` | 4000 ms | 200 ms | linear | `inventory-reel` |
| `figure-eight` | `world-orbit` | 3000 ms | 100 ms | ease-out | `inventory-reel` |
| `helix` | `world-orbit` | 3200 ms | 120 ms | linear | `inventory-reel` |
| `spiral` | `world-orbit` | 2800 ms | 100 ms | ease-out | `inventory-reel` |
| `vortex` | `world-orbit` | 3600 ms | 90 ms | ease-in | `inventory-turn` |
| `wave` | `world-orbit` | 2400 ms | 80 ms | linear | `inventory-reel` |
| `pillar` | `world-pillar` | 3600 ms | 120 ms | ease-out | `inventory-reel` |
| `bounce` | `world-pillar` | 2600 ms | 70 ms | ease-out | `inventory-turn` |

`crown`, `figure-eight`, `helix`, `spiral`, `vortex`, `wave` and `bounce` draw their shape with a `path`; the others use
the built-in motion of their kind.

## A faithful reel

With `decoys = []`, the faces that scroll past are drawn from the crate's own rewards, each as often as its weight. A
rare reward does not flash past as often as a common one, because it is not as likely. Name materials in `decoys` and
the reel shows those instead. The final face is always the reward already decided.

## Writing your own

Copy a shipped file and change its numbers. Every key is on [Animation files](../config/animation-files.md),
including the `path` expressions of a world reveal and the `window` block that lays out a reveal window of your own.

## The window panes

`opening.window` in [opening.conf](../config/opening-conf.md) sets the `fill` pane (grey) in every uncovered slot of a
reel, turn or pick window, and the `pointer` pane (lime) above and below the slot the reel and the turn stop on. `""`
turns either off. A window an animation file draws puts its own pointer in its `decor`.

Related: [Animation files](../config/animation-files.md), [Opening](opening.md)
