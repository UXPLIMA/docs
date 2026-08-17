---
title: Following and power
order: 4
description: Equip caps, auto-equip, how power adds up, and the thirty follow settings.
icon: footprints
---

## Equipping

```lua
MaxEquipped = 8,
BackpackSize = 500,
```

The base caps, before bonuses. A player's real caps are:

```
equip cap    = MaxEquipped + gamepass bonuses + index goal bonus + API bonus
backpack cap = BackpackSize + gamepass bonuses + API bonus
```

Both are published as player attributes, `MaxEquipped` and `BackpackSize`, so your own
scripts can read them.

Equipping is refused if the pet is not owned or the cap is reached. Deleting is refused if
the pet is locked.

## Auto-equip

```lua
AutoEquip = { FillEmptySlots = true, ReplaceWeaker = true },
```

| Setting | Effect on a new pet |
|---|---|
| `FillEmptySlots` | Equips it if a slot is free |
| `ReplaceWeaker` | If slots are full, swaps out a weaker pet for it |

Both on is the modern simulator behaviour: a player never has to open the menu to benefit
from a better hatch.

Turn `ReplaceWeaker` off if your game wants players to curate their squad deliberately, and
both off if equipping is meant to be a decision.

Auto-equip runs on hatching and on a successful merge.

## Stacking

```lua
PetStack = true,
```

Identical pets are shown as one box with a count. `false` gives every pet its own box.

This is presentation only. Each pet is still a separate entry with its own id, lock state
and equipped state.

Leave it on. A player with 300 pets and no stacking has an unusable inventory.

## Power

Each pet's power is:

```
power = baseBoost * variant.boostMultiplier
```

The equipped squad's powers combine into one number:

```lua
BoostMode = "sum",
```

| Mode | Result |
|---|---|
| `"sum"` | Added together. Eight pets at 10 give 80 |
| `"product"` | Multiplied. Eight pets at 10 give 100,000,000 |

<Callout type="warning" title="Product mode escalates very fast">

Multiplying eight values means a squad upgrade compounds. Two pets going from 10 to 25
takes a squad from 80 to 110 in sum mode and from 100 million to 625 million in product
mode.

Sum is the right default. Choose product only if your entire economy is built around
numbers that large.

A pet with a power of 1 in product mode is also worth noting: it contributes nothing, and a
power of 0 zeroes the whole squad.

</Callout>

The result is written to the player's **`PetBoost`** attribute and is what damages
breakables.

## Mystery pets in the total

Mystery pets are resolved before combining: each takes the value of the strongest equipped
non-mystery pet. See [Pets, rarities and variants](pets.md).

## Following

`Config/Follow.luau` is around thirty numbers describing how the squad trails behind the
player. They are feel settings: change one, save, watch.

### The formation

```lua
Spacing = 4.5,
RowDepth = 4.0,
FirstRow = 4.0,
```

A grid behind the player: `Spacing` sideways, `RowDepth` between rows, `FirstRow` from the
player to the front row.

Raise all three together for large pets, lower them for a tight cluster.

```lua
Separation = 3.0,
SeparationIterations = 2,
InwardAngle = 15,
```

`Separation` stops two pets overlapping, at the cost of a little grid accuracy. `0` turns it
off. `InwardAngle` turns the outer pets slightly toward the centre so the squad reads as a
group rather than a rank.

### Catching up

```lua
FollowMin = 2.5,
FollowMax = 8.0,
TurnSpeed = 9.0,
```

Each pet picks its own speed between the minimum and maximum, so they trail unevenly and
look alive rather than welded to a grid.

Narrow the gap for a disciplined formation; widen it for a scattered crowd. Higher values
overall mean tighter following and less lag on turns.

### Movement

```lua
MoveThreshold = 2.0,
WalkBlendSpeed = 8.0,
BobFreq = 9.0,
IdleBob = 0.06,
```

Below `MoveThreshold` a pet idles and hovers gently instead of walking. `WalkBlendSpeed`
controls how smoothly it crosses between the two.

Ground pets hop:

```lua
HopFreq = 7.0,
HopHeight = 1.6,
WalkLean = 0.12,
```

Flyers float and bank instead:

```lua
FlyBobFreq = 6.0,
FlyBobAmp = 0.5,
FlyRoll = 0.22,
FlyPitchNod = 0.08,
FlyForwardLean = 0.1,
```

Which set a pet uses comes from its `flying` field in `Config/Pets.luau`.

### If your models face the wrong way

```lua
FaceYawOffset = 0,
```

Set it to `180` if your pets run backwards. That is a model orientation problem, and this is
the one-line fix rather than re-exporting every model.

`AttackPitch` and `FlyForwardLean` can likewise be negated if a head tilts the wrong way.

### Attack mode

```lua
AttackWalkSpeed = 16,
AttackJump = 1.8,
AttackInset = 2,
AttackPitch = -0.7,
AttackLeapFraction = 0.28,
```

When sent to a breakable, pets walk over and leap onto it repeatedly. `AttackInset` keeps
them from all landing on the same point, and `AttackLeapFraction` is how much of each hit
cycle is the leap rather than the wait.

See [Breakables and zones](breakables.md).

### Ground detection

```lua
ClimbTolerance = 14,
RayDistance = 500,
```

How tall a step pets can follow up, and how far down the ground ray reaches. Raise
`RayDistance` if your map has very tall drops; otherwise leave both alone.

## Where pets live

Spawned pet models are parented to `Workspace/uxrPetSystemWorkspace/Spawned`, which the
bootstrap creates and clears on every server start.

Equipped squads are broadcast to every client, so players see each other's pets.
