---
title: Types
order: 302
description: What an egg type is, the two shipped types, repeatable eggs, and adding or removing a type.
icon: shapes
---

A type is what an egg is. Each one is a file in `types/`, and its name is the file name: `types/golden.conf` is the
type `golden`. Every key of a type file is on [Type files](../config/type-files.md).

## The shipped types

| Type | Name | Reward | Particle | Effects |
|---|---|---|---|---|
| `common` | *Common Egg* | 50 experience points | 3 `HAPPY_VILLAGER` | A pickup sound, *You found a Common Egg*, the count on the action bar |
| `golden` | *Golden Egg* | 500 experience points | 5 `END_ROD` | A level-up sound, *You found a Golden Egg*, a title *Golden Egg* with *Well hidden* under it |

Both are found once and never again.

## Repeatable eggs

```hocon
cooldown = "1d"
```

A type with a cooldown can be found again by the same player once the time has passed since their last find of that
egg. Each repeat find pays the type's reward and plays its effects; it does not count again towards milestones,
completion or the board. Inside the cooldown, a click answers *This egg is ready again in 23h 10m.* An egg the player
has found once stops drawing idle particles and radar hints for them, even when it is ready again.

A time is written with `d`, `h`, `m` and `s`: `30m`, `12h`, `1d 12h`. A cooldown the plugin cannot read is treated as
empty, so the egg is found once only, and the console says so.

## Adding a type

1. Copy `types/common.conf` to `types/<name>.conf`. The name holds lower-case letters, digits, `-` and `_`, up to 32.
2. Change what it says: the name, the head, the reward, the particle, the effects.
3. For a name players read in their own language, add `type.<name>.name` to every messages file and write
   `display-name = "@type.<name>.name"`.
4. Run `/uxmeastereggs reload`. The type is in the picker and in `create`.

A file whose name is not a valid type name, or which cannot be read, is skipped with the reason in the console; the
other types load.

## Removing a type

Delete its file and reload. Its eggs stay on the map and answer *This egg has no type* until you change their type
with `/uxmeastereggs set <egg> type <type>` or put the file back.

The two shipped types are written back at every start when their file is missing, so `common` and `golden` return
after a restart. To retire one of them, keep the file and place no egg of it; it then shows in the collection as
*Found 0 of 0*.

## The collection

The [collection](../menus/collection.md) has one tile per type, not per egg: how many of that type's eggs the player
has found, out of how many are placed, and whether they have found them all.

Related: [Type files](../config/type-files.md), [Rewards](rewards.md), [Radar and particles](radar-and-particles.md)
