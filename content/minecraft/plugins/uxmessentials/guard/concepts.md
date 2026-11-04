---
title: Concepts
order: 1040
description: Regions, shapes, priority, parents, families, roles, and how guard decides an action.
icon: lightbulb
---

Contents: [Regions](#regions) · [Shapes](#shapes) · [Rules and behaviours](#rules-and-behaviours) ·
[Roles](#roles) · [Overlapping regions](#overlapping-regions) · [Parents and families](#parents-and-families) ·
[Outside every region](#outside-every-region) · [Locked regions and locked worlds](#locked-regions-and-locked-worlds)

## Regions

A region is a named area of one world. Its name is 1 to 64 characters of lower-case letters, digits, `_` and
`-`, starting with a letter or digit; what you type is lower-cased, so `Spawn` and `spawn` are one region. Two
worlds may each have a region with the same name, so a command that takes a region also takes `world:name`;
without the world it means the world you stand in.

Every region is stored in the uxmEssentials database, not in a file, and every change to it is written to its
history so it can be [undone](tools.md#history-and-undo).

| Part | What it is |
|---|---|
| Shape | The area, see [Shapes](#shapes) |
| Rules | Who may do which [action](actions.md) inside it, see [Rules](rules.md) |
| Behaviours | What it does to the players inside it, see [Behaviours](behaviours.md) |
| Roles | Players with a role in this region: owner, manager, member, guest, or a role of your own |
| Priority | A whole number, `0` by default. The higher number wins where regions overlap |
| Parent | Another region whose rules, roles, behaviours and template this one inherits |
| Family | A name that lets regions trade blocks across their borders, see [Parents and families](#parents-and-families) |
| Template | A ready rule set from `modules/guard/config.conf`, see [Templates](templates.md) |
| Tags | Free words for grouping, so one command can edit every region with a tag |
| Scope | Which servers load the region, see [Networks](networks.md) |
| Locked | Whether staff without the lock node may change it, see [below](#locked-regions-and-locked-worlds) |

**A new region does nothing.** It has no rules, so nothing in it is refused until you add one or link a
template. That is on purpose: a region you are still drawing never locks anyone out.

## Shapes

`/guard define <name> [shape]` makes a region from the shape you name. Without a shape it is a cuboid from
your selection.

| Shape | Typed as | Made from |
|---|---|---|
| Cuboid | `cuboid` (`kup`) | Your selection: two corners from the guard wand or `/guard pos1` and `pos2`, or your WorldEdit or FAWE selection when one is installed |
| Polygon | `polygon [minY] [maxY]` (`poligon`) | The points you added with sneak and right click of the wand or `/guard point`, from `minY` to `maxY` (the whole world height by default) |
| Cylinder | `cylinder <radius> [height]` (`silindir`) | A circle around where you stand. Without a height it spans the whole world height; with one it starts at your feet |
| Sphere | `sphere <radius>` (`kure`) | A ball around where you stand |
| Chunks | `chunks [radius]` (`chunk`) | The chunk you stand in, and that many chunks around it (up to 32), full height |
| Global | `global` (`dunya`) | The whole world |

`/guard redefine <region> [shape]` gives an existing region a new shape and keeps everything else.

The guard wand is a blaze rod from `/guard wand`. Left click a block for the first corner, right click for the
second, sneak and right click to add a polygon point. `/guard clear` forgets your selection and
`/guard expand <amount> <direction>` grows it (`up`, `down`, `north`, `south`, `east`, `west` or `all`;
`/guard expand vert` stretches it to the full world height).

## Rules and behaviours

A **rule** answers one question: may this player do this action here. It is written as
`<action> <target> <allow|deny>`, for example `break nonmember deny` or `use.door everyone allow`. Actions form a
tree, so a rule on `use` covers every `use.*` action that has no rule of its own. [Rules](rules.md) explains how
rules are weighed.

A **behaviour** is not a yes or no. It is a setting the region applies to the players inside it, like
`fly = deny`, `message.enter = <green>Welcome` or `heal = 2 5`. When a player leaves, guard puts back what it
changed. [Behaviours](behaviours.md) lists them all.

## Roles

A role is a player's place in one region. Four are built in, and `roles.extra` in the config adds more.

| Role | Meaning |
|---|---|
| `owner` | Owns the region. May hand out `member` and `guest` in it with `/guard member` or the roles window |
| `manager` | Runs the region for the owner. The same roster rights as an owner |
| `member` | Belongs to the region. Rules for `nonmember` do not apply to them |
| `guest` | A visitor with a role. Rules for `nonmember` do not apply to them either |

Roles only mean something through rules: `break nonmember deny` lets every role build, `break role:member allow`
names one role. Everything beyond the member roster (shape, rules, behaviours, other roles) is staff work.

## Overlapping regions

Where regions overlap, guard asks them in order and the first one that has an answer for the action decides:

1. The higher priority first.
2. With equal priority, the region inside the other first: a shop inside a town speaks before the town.
3. Equal priority and neither inside the other: the two speak together, and if one allows and the other denies,
   deny wins.
4. Last, the world defaults from the config.

A region with no rule for the action does not answer, and the next one is asked. If nobody answers, the action
is allowed. [Rules](rules.md#across-regions) has worked examples.

## Parents and families

A **parent** shares its setup. `/guard parent shop town` makes `shop` inherit the rules, roles, behaviours and
template of `town`; anything `shop` sets itself overrides what it inherited. Parents chain, so a region can sit
under a region under a region. The chain is resolved once when the region is compiled, so a deep chain costs
nothing per check.

A **family** controls borders. Water, lava, fire, pistons, growth, explosions, hoppers and dispensers move
blocks or items from one place to another. When the source and the target are in regions of different
families, the move is refused, so a neighbour cannot flood, burn or drain your region from outside. A region's
family is its top parent's name unless you set one with `/guard family <region> <name>`. Regions that should
trade freely (a market and its stalls) share a family. This border test runs in any world that has at least one
rule.

## Outside every region

Where no region exists, guard answers in a few nanoseconds and allows everything, unless the world defaults in
`worlds` of the config say otherwise. A chunk with no region in it costs no memory at all.

## Locked regions and locked worlds

`/guard lock <region>` marks a region as finished. Its rules still apply as before, and:

- changing it in any way needs `uxmessentials.guard.lock`, even for staff who may edit other regions;
- `/guard bypass` does not work inside it for anyone without that node;
- with WorldEdit or FAWE installed, an edit whose selection touches it is refused.

A **locked world** is different: it is what guard does when it cannot read a world's regions from the database
at start. Rather than run that world unprotected, it refuses every player action and every nature action
there, except for players with bypass on, and tells staff with `uxmessentials.guard.notify` when they join.
`/guard status` shows `locked true`. Fix the database and restart, or run `/guard reload`.
