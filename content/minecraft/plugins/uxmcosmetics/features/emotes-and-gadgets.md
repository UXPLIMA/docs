---
title: Emotes and gadgets
order: 309
description: Bursts of particles, sounds and faces played on a click, cooldowns, and the 44 gadget actions.
icon: party-popper
---

Emotes and gadgets are **bursts**: steps of particles, a sound and, for an emote, a face over the head. Nothing is
written and nothing blocks.

## Emotes

An emote is played from its family window: a click on an emote the player has plays it, every time. `/cosmetics
equip` selects it; the window plays it.

| Key | Meaning |
|---|---|
| `steps` | The figures, in order: `{ particle {...}, figure, radius, height, delay-ms }` |
| `figure` | `POINT`, `RING`, `SPHERE` or `CONE` |
| `bubble.item` | What floats over the head: a `PLAYER_HEAD` with the face's `skull-texture` |
| `bubble.ms`, `bubble.height` | How long it stays, and how high (3.0 clears a standing player's hat) |
| `sound`, `volume`, `pitch` | The sound |
| `cooldown-ms` | The wait before it can be played again |

## Gadgets

A gadget puts a marked item in `gadget.slot`; a right click plays it. The item cannot be moved, dropped or placed and
is taken back when the gadget comes off or the player leaves.

| Key | Meaning |
|---|---|
| `action` | What the gadget does: the scene it builds, the thing it fires, or the answer it gives. Keep the one a shipped gadget names |
| `item` | The hotbar item |
| `steps`, `sound`, `volume`, `pitch` | As for an emote |
| `push { forward, up }` | Throws the player on the first step, in blocks per tick |
| `cooldown-ms` | The wait, shown as the white bar over the item |

A gadget whose action moves the player must not also carry `push`: push runs after the action and would overwrite it.

### What gadgets do

- **Scenes** paint blocks on the clients of everyone who can see them and paint them back a few seconds later: the
  world never changes, and a player may stand on one without being kicked for flying. A click on a painted block keeps
  it painted.
- **Shots** fire a real projectile, such as TNT or a wither skull, whose blast is cancelled and drawn as a show.
- **Answers** say a line: the magic ball picks one of `gadget.magic-ball.1` to `8`, the fortune cookie one of
  `gadget.fortune.1` to `5`, as `gadget.says`.

A scene that has no room answers `gadget.no-room`. Every gadget and its action is on [Gadgets](../catalogue/gadget.md).

## Answers

| Answer | When |
|---|---|
| `burst.cooldown` | Within the cooldown, with the seconds left |
| `burst.not-allowed` | The node or ownership was lost since it was put on |
| `burst.nothing` | Nothing is equipped in the family |

Related: [Category files](../config/category-files.md#burst-families), [Catalogue](../catalogue/emote.md)
