---
title: Concepts
order: 203
description: Arenas and their states, how a player gets in and out, the crown and a reign, the record, and the boards.
icon: lightbulb
---

Five ideas carry the whole plugin: the arena, the session, the crown, the record and the boards.

## Arenas

An arena is a file in `arenas/`, named after its id. It holds five pieces and a few switches:

| Piece | What it is |
|---|---|
| `corner1`, `corner2` | Two opposite corners of the box, in any order. A player is in the arena while their feet are inside it |
| `plate` | The block a king stands in. It must be inside the box |
| `spawn` | Where `/kotl join` puts a player, and where a player who dies inside comes back |
| `exit` | Where `/kotl leave` puts a player. It may be in another world |

| Switch | Default for a new arena | What it does |
|---|---|---|
| `enabled` | off | Whether players can get in |
| `join-on-enter` | on | Whether walking into the box joins |
| `clear-inventory` | on | Whether the items are put away on entry and given back on leaving |
| `clear-effects` | on | Whether potion effects are removed on entry. They are not given back |
| `game-mode` | adventure | The game mode a player is put in; the old one comes back on leaving |

An arena is in one of three states:

| State | Means | Can players get in |
|---|---|---|
| **incomplete** | A piece is not set, or the plate is outside the box | No. `enable` is refused and names what is missing |
| **off** | Every piece is set, and an operator switched it off | No |
| **on** | Every piece is set and it is switched on | Yes |

An edit that takes a piece away from an open arena, such as moving a corner so the plate falls outside the box,
switches the arena off at once and says why. A new arena starts empty and off. See [Arenas](../arenas/overview.md).

## Sessions

A player is in a session from the moment they get in until they get out. At most one arena at a time.

| Way in | Needs | Where the player stands |
|---|---|---|
| Walking into the box | `join-on-enter`, the arena on, and a step from a block outside the box to one inside | Where they walked in |
| `/kotl join <arena>` | The arena on, and `uxmkotl.play` | Teleported to the spawn |

| Way out | What happens |
|---|---|
| `/kotl leave` | Items and game mode back, teleported to the exit |
| Walking out of the box | Items and game mode back, where they stand |
| Quitting | The reign ends now; items and game mode come back when they join again |
| The server stopping cleanly | Everybody inside gets their items and game mode back and goes to the exit |

Dying is not a way out: a player who dies inside keeps everything and comes back at the arena's spawn, still in the
session. See [Fights](../arenas/fights.md).

## The crown

Each arena has one crown. The player whose feet are in the plate block holds it.

1. **A free plate** is claimed the moment a player steps into it.
2. **A sitting king** is protected for `crown.claim-cooldown-ms` (1500) after their claim. A player stepping onto the
   plate then is refused and told how long is left; with `plate.repel` on, they are pushed off.
3. **After the protection,** a player who steps into the plate block takes the crown, and the old king's reign ends.
4. **The reign ends** when the king steps off the plate, steps out of the box, leaves, quits, dies, or loses the crown
   to a new claim.

A reign's length is the time between its claim and its end. See [The crown](../arenas/crown.md).

## The record

Every player has one record, shared by all arenas:

| Figure | Grows when |
|---|---|
| Time held | A reign ends: its length is added |
| Longest reign | A reign ends that is longer than any before |
| Crowns | A player claims the crown |
| Kills | A player in an arena dies to another player in the same arena |

`/kotl stats` shows it. A reign that is still running when the server stops is not added.

## Boards

Four boards, each kept for up to four periods:

| Board | Ranks by | Shown as |
|---|---|---|
| `held` | Time held in the period, added up | a time, such as `4m 12s` |
| `longest` | The longest single reign in the period | a time |
| `claims` | Crowns taken in the period | a count |
| `kills` | Kills in the period | a count |

| Period | Starts again |
|---|---|
| `alltime` | never |
| `monthly` | on the first day of the month |
| `weekly` | on `leaderboard.week-starts`, Monday by default |
| `daily` | at midnight in `leaderboard.time-zone` |

The boards are read again every `leaderboard.refresh-seconds` (15) and answered from memory, so a hologram can ask for
them as often as it likes. `/kotl top` and the placeholders read them; the leaderboard window ranks the all time
record by time held. See [Placeholders](../placeholders/reference.md).

Related: [Your first arena](first-arena.md), [Arenas](../arenas/overview.md), [config.conf](../config/config-conf.md)
