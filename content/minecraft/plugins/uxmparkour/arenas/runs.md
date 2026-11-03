---
title: Runs
order: 404
description: Joining, the session items, what is locked while a player runs, every way a run ends, and arena protection.
icon: activity
---

A run is one player's attempt in one arena, from the moment they join to the moment they are put back. One player
runs one arena at a time.

## Joining

`/parkour`, `/parkour join [arena]`, or a command or NPC that runs one of them for the player. In order:

1. The difficulty and theme are decided: the player's own choice, then the arena's, then the defaults
   ([Concepts](../getting-started/concepts.md#what-a-player-chooses)).
2. A new seed is drawn and the first `run.blocks-ahead` blocks are placed.
3. Where the player stands, with the way they face, is remembered.
4. Their inventory is taken and held, and the session items are put in.
5. They are moved onto the start block.

Experience, health, hunger, game mode and potion effects are not touched, and nothing of them is given back at
the end.

## The session items

Two items sit in the hotbar during a run. They are set in `session-items` in
[`config.conf`](../config/config-conf.md#session-items).

| Item | Slot | Material | Right click |
|---|---|---|---|
| Restart | `3` (the fourth hotbar slot) | `CLOCK` | The same as `/parkour restart` |
| Leave | `5` (the sixth) | `RED_DYE` | The same as `/parkour leave` |

An item is recognised by the slot it sits in, so changing the material changes nothing else. Its name and lore are
`item.restart` and `item.leave` in the messages files. `enabled = false` leaves an item out.

## While running

| Locked | Why |
|---|---|
| All damage | A fall into the void or a mob cannot end a run any way but the fall rule |
| Dropping an item | The held inventory comes back whole |
| Clicking in the inventory | The session items stay where they are |
| Swapping hands | The same |

Commands are not locked. A teleport by another plugin does not end the run by itself: it counts as a move, so a
player sent somewhere lower than the fall rule allows has fallen, and a player sent higher stays in the run until
they leave, fall or disconnect. Block the teleport commands for runners, or end the run first with
`parkour leave` run as the player.

## How a run ends

| Ending | Caused by | What the player is told | Back to where they joined | Counts |
|---|---|---|---|---|
| Fall | Dropping more than `run.max-fall` below the block last stood on | The `fell` moment, then their best or the `personal-best` moment | yes | yes |
| Exhausted | No jump fits anywhere in the box, and the blocks ahead are used up | The `exhausted` moment, then their best or the `personal-best` moment | yes | yes |
| Leave | `/parkour leave` or the leave item | `run.left`, with the jumps | yes | yes |
| Restart | `/parkour restart` or the restart item | `run.restarted`, with the jumps of the old run | no: a new run starts on the start block | yes |
| Disconnect | The player leaves the server | nothing | yes, the inventory is back before they are saved | yes |
| Shutdown | The server stops, or an operator disables or deletes any arena | nothing | yes | yes |

The fall rule is measured from the block the player last stood on, not from the floor of the arena, so the same
miss ends a run near the top of a tall box and near the bottom. With the default `max-fall = 6`, a player whose feet
are more than six blocks below their last block has fallen.

Every ending removes the run's blocks first, then hands the inventory back, then moves the player. A restart keeps
the inventory held, so nothing is handed back and taken again in the same moment.

## What counts

| Record | A run adds |
|---|---|
| Best run (`/parkour stats`, the `best` board) | Its jumps, when higher |
| Total jumps (the `jumps` board) | Its jumps |
| Total runs (`/parkour stats`) | One, even with no jump |
| The `runs` board | One, only when the run had at least one jump |

A personal best is a run longer than the player's best before it. It plays the `personal-best` moment only on a fall
or an exhaustion; a run left by command still counts as the new best.

## Protection

With `run.protect-arena = true` (the default), nobody may break or place a block inside the box of an arena while a
run is in it. `uxmparkour.admin.build` (op) is never stopped. An arena nobody is running is not protected, so build
around it then, or use a protection plugin for the space around the box.

<Callout type="danger" title="A crash during a run loses the inventory">

A player's inventory is held in the server's memory for the length of the run. A clean stop gives it back first; a
crash, a kill of the Java process or a power cut does not, and the player comes back with the session items only.
Keep valuable inventories out of the lobby, or back the server up with a tool that restores player data.

</Callout>

## Notes

- **A player with nothing to give back.** A run that ended without a session still checks for a held inventory and
  returns it, so a run interrupted halfway does not keep a player's items.
- **Returning.** The player is put back with the exact position and facing they joined from, through an async
  teleport, which is what Folia needs.
- **One run per player.** `/parkour join` while running answers `run.already-running` with the arena's name.

Related: [Moments](moments.md), [Rewards](rewards.md), [Player commands](../commands/player.md)
