---
title: Auto Kill
order: 33
description: Killing configured mobs, natural and spawner switches, and what a kill pays.
icon: swords
---

Auto Kill kills the mobs you list and gives the farmer their drops. It turns a mob farm into a
production line, and it is the module most likely to need limiting.

```yaml
auto-kill:
  name: "Automatic Kill"
  status: true
  default-status: false
  force-default: false
  permission-required: true
  only-kill-spawner-mobs: false
  mobs:
    - SHEEP
    - COW
    - IRON_GOLEM
```

| Option | What it does |
|---|---|
| `status` | Whether the module exists on this server |
| `default-status` | Whether a new farmer has it switched on |
| `force-default` | Holds every farmer at `default-status`, see [modules.yml](../config/modules-yml.md) |
| `permission-required` | Requires the owner to hold `uxmfarmer.autokill` |
| `only-kill-spawner-mobs` | Never kills naturally spawned mobs, on any farmer |
| `mobs` | Every entity type the module may kill |

## Which mobs

`mobs` is the whole list. Anything not in it is safe from the farmer no matter what a player does,
which makes this the one setting to get right before opening the module to players. Adding
`IRON_GOLEM` (as the default does) means iron farms work; adding a hostile mob means the farmer
does the fighting.

Players can narrow the list further in their own auto-kill settings menu, but never widen it.

## Natural and spawner mobs

A farmer has two switches, which players toggle themselves:

| Switch | Kills |
|---|---|
| `AUTO_KILL` | Listed mobs that spawn naturally, and iron golems villagers call in |
| `AUTO_KILL_SPAWNER` | Listed mobs that come out of a spawner |

`only-kill-spawner-mobs: true` leaves only the second one working, which is how you make spawners
the only source of mob drops.

Some mobs are never killed, whatever the switches say: mobs that were bred, hatched from an egg,
spawned from a spawn egg or a dispenser, and golems, snow golems and withers a player built. A pen
of animals a player breeds stays a pen.

## What a kill pays

| Mob | The farmer gets |
|---|---|
| An adult | Its loot, rolled as if killed without Looting, and its XP |
| A baby | Nothing |
| A slime or magma cube | The loot and XP of every size it would split into |
| A stack | The loot and XP of every mob in it |

Loot that is not a product in `collected-materials.yml` is not collected. XP goes to the farmer when
its XP Collector is on and its owner holds `uxmfarmer.xpcollector`; otherwise it drops as an orb.

## Modifying the settings

Changing auto-kill settings needs `uxmfarmer.autokill.modify` in addition to `uxmfarmer.autokill`.
Splitting them lets you give a rank the module while keeping its mob list fixed at whatever you set.

## Player drops

Drops from mobs a *player* killed are governed separately by `collect-player-drops`:

```yaml
collect-player-dropped-items: true
collect-player-dropped-items-by-default: true
```

and the owner permission `uxmfarmer.collectplayerdrops`. Leave this on and a player fighting in
their own base feeds their farmer; turn it off and only the farmer's own kills count.

<Callout type="warning" title="Auto Kill and stacker plugins">

With a stacking plugin, one spawn can be dozens of mobs and the farmer is paid for all of them, so
tune prices for the stacked output. RoseStacker needs `spawn-into-nearby-stacks: false`: with the
default `true`, a stack is paid as one mob and stacks forming near a spawner are never killed.

</Callout>
