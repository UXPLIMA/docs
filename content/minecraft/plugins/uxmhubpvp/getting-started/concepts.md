---
title: Concepts
order: 103
description: The three states a player is in, what entering and leaving do to the inventory, tags, the record, and the boards.
icon: lightbulb
---

Five ideas carry the whole plugin: the state, the safe inventory, the combat tag, the record and the boards.

## The state

Every player is in one of three states.

| State | Means | Can hit and be hit by |
|---|---|---|
| **Off** | The normal lobby player | nobody, while `arena.only-fighters-may-fight` is on |
| **Warming up** | Standing still before a switch, in or out | as in the state they are leaving |
| **On** | A fighter, holding the kit | other fighters |

One press of the toggle item or `/pvp` moves between them:

| Pressed while | Does |
|---|---|
| Off | Starts the warm-up in, `warm-up.enter-ms` (3 seconds) |
| On | Starts the warm-up out, `warm-up.leave-ms` (8 seconds) |
| Warming up | Stops the warm-up; the player stays as they were |

Moving more than `warm-up.move-limit` (1.5) blocks from where the warm-up started stops it too. The way out is
longer than the way in on purpose: a player who is losing cannot step out the moment the fight turns. See
[Turning PvP on and off](../fighting/overview.md).

## The safe inventory

Turning PvP on takes the player's whole inventory, armour and off hand included, writes it to the database, and only
then hands out the kit. If the write fails, the items go straight back and PvP stays off. Leaving, by the toggle, by a
death or by quitting, gives the items back and only then deletes the saved copy.

If the server stops hard while a player fights, the copy is still in the database, and the player gets their items
back the next time they join, with the line `pvp.recovered`. On a network sharing one database, that can be any
server of the network. See [The kit](../fighting/kit.md).

## The combat tag

A hit between two fighters tags both of them for `combat-tag.duration-ms` (10 seconds), and every further hit starts
the time again. While tagged:

- only the commands in `combat-tag.allowed-commands` run;
- quitting counts as a death on the record.

The tag ends when its time runs out or the player dies. See [Combat tag](../fighting/combat-tag.md).

## The record

Every player has one record:

| Figure | Grows when |
|---|---|
| Kills | They kill another fighter |
| Deaths | They die while fighting, or quit while tagged |
| Ratio | Kills divided by deaths, two decimals; kills alone while deaths is 0 |
| Streak | Each kill; a death sets it back to 0 |
| Best streak | Their streak passes it. It never goes down |

`/pvp stats` shows it. A death while fighting also ends the fight: the player respawns with PvP off and their own items.

## Boards

| Board | Ranks by |
|---|---|
| `kills` | Kills in the period |
| `streak` | The longest streak reached in the period |

Each is kept for `alltime`, `monthly`, `weekly` and `daily`. The boards are read again every
`leaderboard.refresh-seconds` (15) and answered from memory, so a hologram can ask for them as often as it likes.
See [Placeholders](../placeholders/reference.md).

Related: [Setting up the fight](first-fight.md), [Turning PvP on and off](../fighting/overview.md), [config.conf](../config/config-conf.md)
