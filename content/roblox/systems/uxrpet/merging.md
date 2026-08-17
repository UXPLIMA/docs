---
title: Merging and the index
order: 6
description: Combining duplicates into rarer variants, the machines, and collection goals.
icon: combine
---

## Merging

```lua
return {
    Normal = { to = "Golden", count = 5, chance = 1 },
    Golden = { to = "Rainbow", count = 5, chance = 1 },
}
```

The key is the variant you put **in**; the entry describes what you get out.

| Field | Meaning |
|---|---|
| `to` | The variant you receive |
| `count` | How many copies a full merge needs |
| `chance` | The success rate at a full count, 0 to 1 |

Five Normals give a Golden, five Goldens give a Rainbow. Both ship at 100 percent.

## Partial merges

You do not have to put in the full count. The success chance scales with how many you
contribute:

```
chance = min(putIn / count, 1) * recipe.chance
```

| Put in | With `count = 5`, `chance = 1` | Result |
|---|---|---|
| 5 | 100 percent | Always works |
| 3 | 60 percent | Often works |
| 1 | 20 percent | Usually fails |

<Callout type="danger" title="A failed merge consumes the pets and gives nothing">

The inputs are removed before the roll, every time. A player who gambles three pets on a 60
percent merge loses all three 40 percent of the time.

That is deliberate and it is also the single most surprising thing about this system for a
new player. Make sure your UI says so, or set `chance = 1` and let the count be the only
cost.

</Callout>

## The rules

A merge is refused if:

| Reason | Means |
|---|---|
| `NotLoaded` | The profile has not loaded yet |
| `NoPets` | Nothing was selected |
| `NotOwned` | A selected pet is not theirs |
| `Duplicate` | The same pet was listed twice |
| `Unavailable` | A selected pet is locked or equipped |
| `Mismatch` | Not every pet is the same species and variant |
| `NoRecipe` | That variant has no merge recipe |
| `TooMany` | More than `count` were selected |

Every pet must be the **same species and the same variant**. Five different Common pets do
not merge into anything.

Locked and equipped pets are excluded, which is the safety net that stops a player merging
away their best squad member.

## The machines

Two world objects open the merge panel, filtered:

| Machine | Merges |
|---|---|
| `GoldenMachine` | Normal into Golden |
| `RainbowMachine` | Golden into Rainbow |

They are found by name anywhere in `Workspace`, as a `Model` or a `BasePart`, and open when
the player walks within `range` studs. See the machine list in
[Breakables and zones](breakables.md).

Adding a third tier means a new recipe, a new machine object, a new panel and a line in
`Config/Machines.luau`.

## Adding a recipe

```lua
Rainbow = { to = "Huge", count = 10, chance = 0.5 },
```

Any variant can merge into any other. A ten-Rainbow merge with a coin-flip chance is a
plausible endgame sink.

Note that a variant used as a merge **input** needs a recipe keyed by its own name, and one
used as an **output** needs to exist in `Config/Variants.luau`, with models if it uses a
prefix.

## The index

The index is the collection log: every species and variant combination that appears in any
egg pool, ticked off as the player hatches them.

A pet enters the index the first time it is obtained. Deleting it does not remove the tick.

The full set is computed from the egg pools, so a pet that exists in `Config/Pets.luau` but
appears in no pool is not part of the index at all. That is the way to add a pet that is
given out by your own scripts without spoiling the collection count.

## Index goals

```lua
Show = true,

Goals = {
    { pets = 5, equipSlots = 1 },
    { pets = 15, equipSlots = 2 },
    { pets = 30, equipSlots = 3 },
},

Format = "Collect {goal} pets for +{slots} equip slots!  ({have}/{goal})",
Maxed = "All equip-slot goals complete!",
```

Collection milestones that grant real extra equip slots.

<Callout type="info" title="The bonus is a total, not a sum">

Reaching 30 collected gives `+3` slots, not `+1 +2 +3 = 6`. The highest goal you have
reached is your bonus.

</Callout>

| Token | Becomes |
|---|---|
| `{goal}` | The target count |
| `{slots}` | The bonus slots |
| `{have}` | How many collected |
| `{need}` | How many more are needed |

`Show = false` hides the line and keeps the bonus working, if you would rather it be a quiet
reward.

Goals should be listed smallest first. The bonus is added on top of the base cap and any
gamepass bonuses.

## Designing goals

Equip slots are the most valuable thing in a pet simulator, so collection goals compete
directly with the equip gamepass.

A workable split: gamepasses sell a large jump early, goals give a slow trickle that keeps
non-paying players progressing. The shipped three goals give three slots for collecting
thirty pets, against three slots for one gamepass.
