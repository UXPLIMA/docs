---
title: Welcome
order: 201
description: What a jump pad is with uxmJumpPads, and what comes with the plugin.
icon: door-open
---

uxmJumpPads throws a player who walks onto a pad. A pad is either every block of a kind sitting on a chosen block, a
light pressure plate on a redstone block for instance, or one block an operator set by hand. The throw is a speed
forward and a speed up, the landing costs no health, and a sound and a puff of particles go with it.

It runs on Paper 26.2 or newer and on Folia, on Java 25. [Installation](installation.md) has the details.

## What a pad is

| Piece | What it means |
|---|---|
| **Pad type** | A kind of pad in `config.conf`: its block, the block under it, how hard it throws, who may use it, and its effects |
| **Matched pad** | Any block of the server that is a type's block on the type's block under it. It throws the way the player faces |
| **Set pad** | One block an operator made a pad with `/uxmjumppads set`. It throws the way the operator faced, whoever steps on it |
| **Throw** | A speed forward and up, given once per arrival on the pad |
| **Fall grace** | The first fall within 10 seconds of a throw costs no health |

[Concepts](concepts.md) explains how the parts fit.

## What comes with it

| Area | What you get |
|---|---|
| Two shipped pads | `classic`, a light pressure plate on a redstone block, and `bounce`, a slime block on an emerald block ([Pad types](../pads/types.md)) |
| Pads anywhere | Every matching block of every world is a pad, with nothing to register |
| Pads on one block | `set` turns the block you stand on into a pad that throws one fixed way ([Set pads](../pads/set-pads.md)) |
| Safe landings | The fall a throw causes costs nothing ([Throws and landings](../pads/throws.md)) |
| Effects | Sounds, particles, messages and commands as action lines ([Effects](../pads/effects.md)) |
| Permissions | A pad type can be kept for players with a node ([Permissions](../permissions/reference.md)) |
| Languages | English, Turkish, German, Spanish, French, Portuguese and Russian, per player ([Languages](../config/languages.md)) |

There is no database and no menu: pad types are settings, and set pads are a small file.

## Where to go next

| You want | Page |
|---|---|
| Get it running | [Installation](installation.md) |
| Understand matched and set pads | [Concepts](concepts.md) |
| Build a pad step by step | [Your first pad](first-pad.md) |
| Write a pad type | [Pad types](../pads/types.md) |
