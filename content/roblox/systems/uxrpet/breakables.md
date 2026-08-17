---
title: Breakables and zones
order: 5
description: Coin piles, damage stages, the share system, rewards and machines.
icon: pickaxe
---

Breakables are the coin piles your pets attack. Clicking one sends part of your squad to
smash it, and it pays out when it breaks.

## A breakable type

```lua
Coins = {
    hp = 2000,
    reward = 1200,
    currency = "Coins",
    respawn = 4,
    hitInterval = 2.5,
    ringRadius = 6,
    damageMult = 1,
    maxRange = 45,
},
```

| Field | Meaning |
|---|---|
| `hp` | Total health |
| `reward` | Total payout, split between everybody who damaged it |
| `currency` | Which currency. Defaults to your main one |
| `respawn` | Seconds until it returns after breaking |
| `hitInterval` | Seconds between each pet's leap. Presentation only |
| `ringRadius` | How wide the pets circle it |
| `damageMult` | Multiplies damage against this type |
| `maxRange` | How far the player may walk before their pets give up |

`damageMult` is the per-type difficulty knob. A late-game breakable with a high `hp` and a
`damageMult` of `0.5` is twice as hard again without touching any player's numbers.

## Damage stages

The models live in `Storage/Breakables/<Type>/` named `<Type>1`, `<Type>2` and upward.

The **highest** number is the full, healthy look. As health drops the model swaps down
through the stages, so `Coins3` is a full pile and `Coins1` is nearly gone.

Two stages work. Ten work. The health range is divided evenly between however many you
provide.

Each stage model needs a part named **`Hitbox`**, which is what the health bar attaches to
and what distance is measured from.

## Health bars

A `BillboardGui` named `HealthBar` in `Storage/Breakables` is cloned onto each breakable.

Inside it, a `Frame` containing another `Frame` for the fill and a `BarTextLabel` for the
number. The fill colour shifts as health drops.

No `HealthBar` means no bars, and everything else works.

## Zones

```lua
Zone1 = {
    spawns = {
        { type = "Coins", count = 8 },
    },
},
```

Each key must match a folder in the world:

```
Workspace
  uxrPetSystemWorkspace
    Zones
      Zone1
        Zone            a flat BasePart covering the area
    Breakables          created for you, cleared every start
```

The breakables are spread across the `Zone` part in a grid and dropped onto the ground with
a downward ray, so an uneven floor still places them sensibly.

Mix types freely in one zone. The total of the counts is how many appear.

<Callout type="info" title="Zones are laid out once, at server start">

The grid is computed when the server starts and the positions do not change. A breakable
respawns in exactly the slot it broke in.

Resizing the `Zone` part at runtime does nothing until the next server start.

</Callout>

## The share system

Clicking a breakable assigns part of your squad's power to it.

| Click | Your share of that breakable |
|---|---|
| First | 0.5 |
| Second | 0.75 |
| Third | 1.0 |

Your total share across all breakables is capped at 1. Raising one target's share above what
is left over takes it proportionally from your other targets.

That is what lets a player split their squad across two coin piles, or commit everything to
one.

## Damage

Every half a second, for each breakable you have a share in:

```
damage = PetBoost * yourShare * damageMult
```

Your `PetBoost` is your equipped squad's combined power. See
[Following and power](following.md).

A target further than `maxRange` from you is dropped from your shares, and your pets come
home. Opening an egg also releases every share.

## Rewards

When a breakable dies, its `reward` is split by **how much damage each player did**:

```
yourCoins = floor(reward * yourDamage / totalDamage)
```

Two players who each did half get half each. A player who did 10 percent gets 10 percent.

That is contribution-based rather than last-hit-based, so helping somebody finish a pile is
worth doing and stealing a kill is not a thing.

Each payer gets a toast, an updated balance, and a `BreakableBroken` signal fires for
scripts of yours that want to react.

<Callout type="warning" title="Reward is a total, not a per-player amount">

`reward = 1200` means 1200 coins are paid out no matter how many players hit it. Ten players
splitting one pile get 120 each.

If you want a pile to be worth 1200 to everybody, that is a change to the payout logic, not
a config value.

</Callout>

## Balancing

The shipped Coins type is a demo:

| | |
|---|---|
| 2,000 health | Roughly a second and a half for an eight-pet squad of Commons |
| 1,200 reward | More than half a Basic Egg |
| 4-second respawn | Effectively continuous income |

For a real game, work backwards: decide how long a player should farm for one egg, then set
`hp` and `reward` from their expected `PetBoost` at that stage.

Add a second type with a higher `hp` and a lower `damageMult` as the next zone.

## Machines

```lua
return {
    { machine = "IndexMachine", panel = "IndexPanel", range = 7 },
    { machine = "GoldenMachine", panel = "GoldenMachinePanel", range = 7 },
    { machine = "RainbowMachine", panel = "RainbowMachinePanel", range = 7 },
}
```

Objects in the world that open a UI panel when a player walks close, with no button to
press.

| Field | Meaning |
|---|---|
| `machine` | The name of the object in `Workspace` |
| `panel` | The frame in the pet ScreenGui it opens |
| `range` | How close, in studs |

Walking away closes it again. Add your own by naming an object and a panel.

The two merge machines open the merge panel filtered to that variant. See
[Merging and the index](merging.md).
