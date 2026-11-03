---
title: Concepts
order: 203
description: The sixteen colours, solid and cycling glows, how a glow is drawn, and why it is global.
icon: lightbulb
---

## Sixteen colours

A client draws an outline in one of sixteen colours and no other. Each has an id and a node:

| Id | Name | Node |
|---|---|---|
| `black` | Black | `uxmglow.colour.black` |
| `dark_blue` | Dark Blue | `uxmglow.colour.dark_blue` |
| `dark_green` | Dark Green | `uxmglow.colour.dark_green` |
| `dark_aqua` | Dark Aqua | `uxmglow.colour.dark_aqua` |
| `dark_red` | Dark Red | `uxmglow.colour.dark_red` |
| `dark_purple` | Dark Purple | `uxmglow.colour.dark_purple` |
| `gold` | Gold | `uxmglow.colour.gold` |
| `gray` | Gray | `uxmglow.colour.gray` |
| `dark_gray` | Dark Gray | `uxmglow.colour.dark_gray` |
| `blue` | Blue | `uxmglow.colour.blue` |
| `green` | Green | `uxmglow.colour.green` |
| `aqua` | Aqua | `uxmglow.colour.aqua` |
| `red` | Red | `uxmglow.colour.red` |
| `light_purple` | Light Purple | `uxmglow.colour.light_purple` |
| `yellow` | Yellow | `uxmglow.colour.yellow` |
| `white` | White | `uxmglow.colour.white` |

A colour's name is its id written out, the same in every language.

## Three modes

| Mode | What the player sees |
|---|---|
| Off | No outline |
| Solid | One colour |
| Cycle | Two to eight colours in turn, each held for the player's interval |

A player has one mode at a time. Choosing a colour replaces a cycle; starting a cycle replaces a colour.

## How a glow is drawn

The client takes the outline's colour from the colour of the scoreboard team that holds the player's name. uxmLib owns
that team and composes it from what each plugin contributes, so a chat tag and a glow live on one name. uxmGlow
contributes a colour and nothing else.

Two things follow:

- **The colour is global.** Every viewer sees the same one.
- **A name carries one colour.** When two plugins both want it, the one that asked last wins and the other is named
  in the log once.

## Remembered and checked

The choice is stored. At join it is read again and every colour in it is checked against the player's nodes: a colour
they no longer hold is not restored, and they read `glow.lapsed`. That is what makes a colour that came with a rank
leave with it.

## One clock

One shared task drives every cycling player, waking every `cycle.tick-ms`. The cost grows with the number of
cycling players, not with the number of colours. A player's interval is rounded up to the next multiple of the tick.

Related: [Colours and cycles](../features/colours-and-cycles.md), [Permissions](../permissions/reference.md)
