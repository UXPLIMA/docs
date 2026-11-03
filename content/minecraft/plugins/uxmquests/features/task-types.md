---
title: Task types
order: 305
description: All fifty four task types, the facts each filters on, triggers, checked and sampled types.
icon: target
---

A task names a `type`; `/uxmquests types` prints every type this server feeds. An option whose name matches a fact the
type reports narrows the task, and a fact the task says nothing about counts every value.

## Types

| Group | Types and the facts each can filter on |
|---|---|
| Blocks | `block-break`, `block-place`, `block-fertilize`, `block-shear`, `block-harvest`, `block-look`: `block`, `world`. `block-change`: `block`, `to`. `block-drop`: `block`, `item`, `world`. `block-loot`, `block-compost`: `item`, `world`. `block-interact`: `block`, `item`, `world` |
| Items | `craft`, `smelt`, `fish`, `item-enchant`, `item-brew`, `item-consume`, `item-pickup`, `item-drop`, `item-break`, `item-damage`, `item-mend`, `item-barter`: `item`, `world`. `item-launch`: `item`, `projectile`, `world`. `item-trade`: `item`, `mob`, `world` |
| Entities | `mob-kill`: `mob`, `world`, `spawn-reason`, `name`, `hostile`, `weapon`, `projectile`. `entity-breed`, `entity-tame`, `entity-shear`, `entity-fish`, `entity-cure`, `entity-hatch`: `mob`, `world`. `entity-milk`, `entity-bucket`: `mob`, `item`, `world`. `entity-damage-deal`: `mob`, `world`, `cause`, in half hearts. `entity-damage-take`: `cause`, `world`, in half hearts. `entity-totem`: `world` |
| The player | `player-death`: `cause`, `world`. `player-xp`, `player-level`, `player-chat`, `player-walk`, `player-swim`, `player-jump`, `player-playtime`: `world`. `player-ride`: `vehicle`, `world`. `player-command`: `command`, `world` |
| Checked | `get`, `location`, `placeholder`, `player-permission` |
| Other | `trigger`. `shop-buy` and `shop-sell` (`item`, `shop`) count only while uxmShop is on the server |

`player-walk` and `player-swim` count blocks, `player-playtime` seconds.

A hunt for zombies that spawned naturally:

```hocon
{ id = hunt, type = mob-kill, mob = ZOMBIE, spawn-reason = [NATURAL], hostile = "true", amount = 10 }
```

## Trigger

`trigger` advances by command and by nothing else:

```
/uxmquests trigger <player> <trigger> <amount>
```

```hocon
{ id = vote, type = trigger, name = daily-vote, amount = 1 }
```

A vote listener, a crate or a command block feeds the quest with no integration on either side.

## Checked types

`get`, `location`, `player-permission` and `placeholder` describe what is the case, not what happens. They are asked
when a board is looked at and again on the sampling schedule.

```hocon
{ id = stock, type = get,               item = DIAMOND, amount = 64 }
{ id = stand, type = location,          world = world, x = 100, y = 64, z = 100, radius = 10, amount = 1 }
{ id = away,  type = location,          world = world, x = 0, y = 64, z = 0, radius = 200, away = true, amount = 1 }
{ id = fly,   type = player-permission, permission = "essentials.fly", amount = 1 }
{ id = rich,  type = placeholder,       placeholder = "%vault_eco_balance%", operator = above, value = "1000", amount = 1 }
```

`operator` is `equals` (the default), `contains`, `starts-with`, `above` or `below`. A checked task never goes back: a
player who held sixty four diamonds and spent them has still held them.

## Sampled types

Walking, swimming, jumping, looking at a block, standing somewhere and playing for a time have no cheap event, so they
are looked at on a schedule; a jump that starts and lands between two samples is not counted.

| Key in `objectives` | Default | Meaning |
|---|---|---|
| `sampling` | true | false: no task runs per player |
| `sample-interval-ticks` | 10 | How often movement is sampled |
| `placeholder-interval-ticks` | 40 | How often checked types are asked |
| `look-distance` | 5 | How far `block-look` reaches |

A server that writes none of these types sets `sampling = false`.

Related: [Anti-glitch](anti-glitch.md), [Quests and tasks](quests-and-tasks.md)
