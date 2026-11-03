---
title: Your first arena
order: 104
description: Build a tower, mark the box, the plate, the spawn and the exit, and open it to players.
icon: flag
---

This walk-through builds the arena `hill` around a tower with a plate on top. Everything here can be done in the
editor window or typed; both are shown.

## 1. Build the tower

uxmKotl does not build anything. Build a tower, a ladder, a hill or a pyramid, and put a block for the king to stand
on at the top. A pressure plate on top reads well, but the plate piece is a position, not a block type: any block
works, air included. Leave room around the tower for the players fighting their way up.

## 2. Create the arena

Stand in the world the arena is in and type:

```
/uxmkotl create hill
```

The arena is written to `arenas/hill.conf`, empty and switched off, and the [editor](../menus/editor.md) opens on
it. An id is one to 32 characters: lower-case letters, digits and `-`, starting with a letter or a digit. From the
console, name the world: `/uxmkotl create hill world`.

## 3. Mark the box

The box is everything a player may stand in while they play. Make it wide enough for the tower and the ground around
it, and tall enough for the top.

| In the editor | Typed |
|---|---|
| Stand at one corner and left click **Corner one**. Or right click it, then click a block | `/uxmkotl set hill corner1 here`, or `/uxmkotl set hill corner1 18 61 -48` |
| The same for **Corner two**, at the opposite corner | `/uxmkotl set hill corner2 28 76 -38` |

While the editor is open, an outline of the box is drawn in particles for you alone, so you see where it lies.

## 4. Mark the plate

Stand on the top of the tower, in the block the king will stand in, and left click **The plate**, or type
`/uxmkotl set hill plate here`. With `here`, the plate is the block your feet are in: standing on a pressure plate
marks the pressure plate. The plate must be inside the box.

## 5. Set the spawn and the exit

| Piece | Where | Set with |
|---|---|---|
| Spawn | Inside the box, at the foot of the tower. Where `/kotl join` puts a player, and where they come back after a death | **The spawn** in the editor, or `set hill spawn here` |
| Exit | Outside the box, where a player who types `/kotl leave` should land | **The exit**, or `set hill exit here` |

Both keep the way you face. Put the spawn inside the box: a player put outside it leaves the arena on their first
step.

## 6. Choose what the arena does to a player

The shipped answers suit a lobby: walking in joins, the inventory and effects are cleared, the mode is adventure.
Change any of them with the switches of the editor or with `set`:

```
/uxmkotl set hill join-on-enter true
/uxmkotl set hill clear-inventory true
/uxmkotl set hill clear-effects true
/uxmkotl set hill mode adventure
```

## 7. Open it

```
/uxmkotl enable hill
```

If a piece is missing, the reply names it. `/uxmkotl show hill` prints every piece. Once it is on, walk into the box
or type `/kotl join hill`, climb, and stand on the plate: the crown is yours.

## The file it wrote

```hocon
boundary { x1 = 18, y1 = 61, z1 = -48, x2 = 28, y2 = 76, z2 = -38 }
display-name = "hill"
enabled = true
entry { clear-effects = true, clear-inventory = true, game-mode = ADVENTURE }
exit { world = world, x = 23.5, y = 62.0, z = -32.5, yaw = 0, pitch = 0 }
join-on-enter = true
plate { x = 23, y = 68, z = -43 }
spawn { world = world, x = 23.5, y = 62.0, z = -38.5, yaw = 180, pitch = 0 }
world = world
```

Every key is explained in [Arena files](../config/arena-files.md).

## Next

- Pay players for the crown and for kills: [Rewards](../arenas/rewards.md).
- Put the boards on a hologram: [Placeholders](../placeholders/reference.md).
- Change what a claim says and sounds like: [Moments](../arenas/moments.md).
