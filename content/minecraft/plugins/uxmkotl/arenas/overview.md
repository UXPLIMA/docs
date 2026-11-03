---
title: Arenas
order: 401
description: What an arena holds, its states, how a player gets in and out, and what entry does to them.
icon: box
---

An arena is a box around a tower, with a plate inside it. Each arena is its own file, `arenas/<id>.conf`, written by
the [operator commands](../commands/admin.md) and the [editor](../menus/editor.md); the keys are on
[Arena files](../config/arena-files.md). A server may have any number of arenas, in any worlds.

## The pieces

| Piece | Kind | Needed | Notes |
|---|---|---|---|
| `corner1`, `corner2` | block | yes | Opposite corners of the box, in any order. Both blocks are inside |
| `plate` | block | yes | Must be inside the box. Any block: the plugin reads the position, not the type |
| `spawn` | place | yes | A position with a facing. Put it inside the box |
| `exit` | place | yes | A position with a facing, in any world |
| `world` | name | yes | The world of the box, the plate and the spawn |
| `display-name` | text | no | The name players read; the id when not set |

## States

| State | Shown in the list | Meaning |
|---|---|---|
| on | *On*, glowing | Every piece set, switched on. Players can get in |
| off | *Off* | Every piece set, switched off |
| incomplete | *Still to set*, with the pieces | A piece missing, or the plate outside the box. `enable` is refused |

A new arena is off and empty. An edit that makes an open arena incomplete switches it off at once and the reply says
why, so an arena is never open in a state nobody can play in. An arena file that has every piece but no `enabled`
key reads as on.

## Joining and leaving

| Way in | When |
|---|---|
| Walking in | The arena is on, `join-on-enter` is true, and the player steps from a block outside the box to a block inside it. A player who is already standing inside when the arena opens joins by walking out and back in |
| `/kotl join <arena>` | The arena is on. The player is teleported to the spawn |

A player is in one arena at a time. `/kotl join` while inside another arena is refused.

| Way out | Items and mode | Where the player ends up | Moments |
|---|---|---|---|
| `/kotl leave` | Given back | The exit | `dethroned` if king, then `left` |
| Stepping out of the box | Given back | Where they stepped | `dethroned` if king, then `left` |
| Quitting | Given back at their next join | Where they quit | none |
| The plugin or the server stopping | Given back | The exit | none |

A teleport out of the box by another plugin, such as `/spawn`, counts as stepping out on the player's next move.

## What entry does

| Step | Controlled by | Given back on leaving |
|---|---|---|
| The game mode is set | `entry.game-mode`, `adventure` by default | Yes, the mode they had |
| The inventory is put away and emptied | `entry.clear-inventory`, on by default | Yes, exactly as it was |
| Potion effects are removed | `entry.clear-effects`, on by default | No |
| The knockback stick is given | always | See [Fights](fights.md#the-knockback-stick) |

The game mode and the items are held in the server's memory while the player is inside: a crash loses them. A clean
stop gives them back first.

## The outline

While an operator has the [editor](../menus/editor.md) open on an arena, the box is drawn around it in particles,
for that operator alone, every `boundary.outline.period-ms` (1000). It follows every corner they set, and one corner
set draws that single block. It is gone when they close the editor. Players in a round see no outline. The outline is
set in [config.conf](../config/config-conf.md#boundary).

## Notes

- **Players inside keep the arena they joined.** An edit, a reload or `disable` reaches a player when they leave and
  come back. `delete` is refused while anybody is inside.
- **The exit may be inside the box.** A player put there by `leave` is not joined again, because joining by walking
  needs a step in from outside.
- **One arena per box.** Two boxes that overlap both admit a walker into the first one found.

Related: [The crown](crown.md), [Arena files](../config/arena-files.md), [Operator commands](../commands/admin.md)
