---
title: Concepts
order: 203
description: The three states, who is important, the world list, the cooldown, and where a choice is kept.
icon: lightbulb
---

## The three states

| State | The viewer sees | Tool, shipped |
|---|---|---|
| `all` | Everybody. Every player starts here | Lime dye |
| `important` | Only the players who hold `uxmplayerhider.important` | Yellow dye |
| `none` | Nobody | Grey dye, glowing |

A right click on the tool, or a bare `/hide`, moves to the next state: `all`, `important`, `none`, and back to `all`.
`/hide state <state>` goes straight to one. See [States](../hiding/states.md).

A hidden player is gone for that viewer alone: from the world and from the tab list. Hiding is one way, so the
hidden player still sees the viewer, and everybody else sees everybody as before.

## Important players

A player is important when they hold `uxmplayerhider.important`. It is `op` by default, so operators stay visible in
the `important` state on a fresh install. Grant it to your staff group, your partners, or a rank. Nothing else makes
a player important: the plugin reads no friends list.

## Worlds

`worlds` in `config.conf` lists the worlds the plugin works in; an empty list is every world. Outside the list a
player sees everybody, the tool is taken away, and the command refuses. Their choice is not forgotten: it is applied
again when they walk back in. See [Worlds and cooldown](../hiding/worlds-and-cooldown.md).

## The cooldown

Two changes must be `cooldown.ms` apart, 2 seconds by default, because every change re-applies the viewer against
every player online. `uxmplayerhider.cooldown.bypass` skips it.

## Where a choice is kept

A player's state is a row in the database, kept only while it is not `all`, so a lobby with ten thousand visitors
and forty hiders keeps forty rows. It is read before the player finishes joining and applied a tick after they
arrive. On a network sharing one database, a change on one server reaches the others within `storage.sync-seconds`.
See [Database](../database/overview.md).

## Effects

Each state has a list of action lines, run when a player moves into it: a title and subtitle, an action bar line and
a sound, by default. The words are lines of the messages files, named with `@`, so they stay translated. See
[Effects](../hiding/effects.md).
