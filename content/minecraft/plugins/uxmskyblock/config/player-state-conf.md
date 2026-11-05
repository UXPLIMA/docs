---
title: player-state.conf
order: 636
description: What a player carries in each kind of world.
icon: backpack
---

What a player carries in each kind of world.

A server that plays several game modes would give a player one inventory, one level and one set of advancements for all of them: a stack of diamonds walks from a classic island onto a fresh Boxed one, and a Boxed box never grows for an advancement the player earned somewhere else. So worlds fall into groups, and a player who crosses from one group to another puts down what they carry in the group they leave and picks up what they had in the group they enter.

What is kept belongs to the player's profile and is written with its inventory, in one step, before the player is given anything. A crash on either side of a crossing leaves the profile whole: nothing is lost or had twice.

Every world belongs to exactly one group, so nothing can be put down for one group and picked up for another. A world no group names belongs to the group of the server's own island world, a lobby or a resource world as well, unless you name it in another group. In the Nether and the End every mode shares, a player carries for the island they stand over: over a Boxed island, their Boxed things.

The file is `plugins/uxmSkyblock/modules/player-state.conf`. Every default below is the shipped value.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Whether anything is kept apart at all. |

## `groups`

The groups, each a name and its worlds. Worlds in one group share what a player carries. Leave it empty for a group of each world a preset makes islands in, so every game mode with a world of its own keeps its own.

```text
groups {
  main = ["world"]
  boxed = ["boxed"]
  realms = ["realms"]
}
```

## `keeps`

What a group keeps apart. Something switched off is carried everywhere.

| Key | Default | What it does |
|---|---|---|
| `inventory` | `true` |  |
| `ender-chest` | `true` |  |
| `experience` | `true` |  |
| `health` | `true` |  |
| `hunger` | `true` |  |
| `effects` | `true` |  |
| `advancements` | `true` | Every advancement but the recipe ones, which unlock the same recipes everywhere. |
