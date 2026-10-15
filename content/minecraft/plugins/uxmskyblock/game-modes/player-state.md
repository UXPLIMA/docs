---
title: What a Player Carries
order: 140
description: How a server that plays several modes keeps each world group's inventory, level,
  effects and advancements apart.
icon: backpack
---

A server that plays several game modes would otherwise give a player one inventory, one level and
one set of advancements for all of them: a stack of diamonds walks from a classic island onto a
fresh Boxed one, and a Boxed box never grows for an advancement earned somewhere else. So worlds
fall into groups. A player who crosses from one group to another puts down what they carry in the
group they leave and picks up what they had in the group they enter.

It is set in `modules/player-state.conf`.

## Groups

```hocon
groups {
    main = ["world"]
    boxed = ["boxed"]
    realms = ["realms"]
}
```

- Worlds in one group share what a player carries.
- Leave `groups` empty for a group of each world a preset makes islands in, so every game mode
  with a world of its own keeps its own.
- Every world belongs to exactly one group. A world no group names belongs to the group of the
  server's own island world, a lobby or a resource world as well, unless you name it in another
  group.
- In the Nether and the End every mode shares, a player carries for the island they stand over:
  over a Boxed island, their Boxed things.

## What is kept apart

Each of these is `true` as shipped. Something switched off is carried everywhere.

| Key | What it keeps apart |
| --- | --- |
| `keeps.inventory` | The inventory |
| `keeps.ender-chest` | The ender chest |
| `keeps.experience` | Experience and level |
| `keeps.health` | Health |
| `keeps.hunger` | Hunger |
| `keeps.effects` | Potion effects |
| `keeps.advancements` | Every advancement but the recipe ones, which unlock the same recipes everywhere |

`enabled = false` keeps nothing apart.

## Safety

What is kept belongs to the player's profile and is written with its inventory, in one step,
before the player is given anything. A crash on either side of a crossing leaves the profile
whole: nothing is lost or had twice.
