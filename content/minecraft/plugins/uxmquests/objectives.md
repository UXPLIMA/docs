---
title: "Objectives"
order: 30
description: "Every task type a quest can ask for, and what each one counts."
---

A task names a `type`. `/uxmquests types` prints every type this server feeds. A task filters on the facts its
type reports: an option whose name matches a fact narrows the task, and a fact the task says nothing about
counts every value. Options are written as described in [quests](quests.md#quests-and-tasks).

Contents: [Types](#types) · [Trigger](#trigger) · [Checked types](#checked-types) · [Sampled types](#sampled-types) ·
[Anti-glitch](#anti-glitch)

## Types

| Group | Types and the facts each can filter on |
|---|---|
| Blocks | `block-break`, `block-place`, `block-fertilize`, `block-shear`, `block-harvest`, `block-look`: `block`, `world`. `block-change`: `block`, `to`. `block-drop`: `block`, `item`, `world`. `block-loot`, `block-compost`: `item`, `world`. `block-interact`: `block`, `item`, `world`. |
| Items | `craft`, `smelt`, `fish`, `item-enchant`, `item-brew`, `item-consume`, `item-pickup`, `item-drop`, `item-break`, `item-damage`, `item-mend`, `item-barter`: `item`, `world`. `item-launch`: `item`, `projectile`, `world`. `item-trade`: `item`, `mob`, `world`. |
| Entities | `mob-kill`: `mob`, `world`, `spawn-reason`, `name`, `hostile`, `weapon`, `projectile`. `entity-breed`, `entity-tame`, `entity-shear`, `entity-fish`, `entity-cure`, `entity-hatch`: `mob`, `world`. `entity-milk`, `entity-bucket`: `mob`, `item`, `world`. `entity-damage-deal`: `mob`, `world`, `cause`, counted in half hearts. `entity-damage-take`: `cause`, `world`, counted in half hearts. `entity-totem`: `world`. |
| The player | `player-death`: `cause`, `world`. `player-xp`, `player-level`, `player-chat`, `player-walk`, `player-swim`, `player-jump`, `player-playtime`: `world`. `player-ride`: `vehicle`, `world`. `player-command`: `command`, `world`. `player-permission`: `permission`. |
| Asked, not told | `get`, `location`, `placeholder`, `player-permission`, see [checked types](#checked-types). |
| Other | `trigger`. `shop-buy` and `shop-sell` (`item`, `shop`) count only while uxmShop is on the server. |

`player-walk` and `player-swim` count blocks, `player-playtime` counts seconds.

A hunt for a hostile zombie that spawned naturally:

```hocon
{ id = hunt, type = mob-kill, mob = ZOMBIE, spawn-reason = [NATURAL], hostile = "true", amount = 10 }
```

## Trigger

`trigger` advances by command and by nothing else:

```
/uxmquests trigger <player> <trigger> <amount>
```

Any plugin that can run a command, such as a vote listener, a crate plugin or a command block, can feed a quest
with no integration on either side.

```hocon
{ id = vote, type = trigger, name = daily-vote, amount = 1 }
```

## Checked types

`get`, `location`, `player-permission` and `placeholder` describe what is the case, not what happens. They are
asked when a board is looked at and again on the sampling schedule.

```hocon
{ id = stock, type = get,               item = DIAMOND, amount = 64 }
{ id = stand, type = location,          world = world, x = 100, y = 64, z = 100, radius = 10, amount = 1 }
{ id = away,  type = location,          world = world, x = 0, y = 64, z = 0, radius = 200, away = true, amount = 1 }
{ id = fly,   type = player-permission, permission = "essentials.fly", amount = 1 }
{ id = rich,  type = placeholder,       placeholder = "%vault_eco_balance%", operator = above, value = "1000", amount = 1 }
```

`operator` is `equals`, `contains`, `starts-with`, `above` or `below`; the default is `equals`. A checked task
never goes backwards: a player who held sixty-four diamonds and then spent them has still held them.

## Sampled types

Walking, swimming, jumping, looking at a block, standing somewhere and playing for a time have no cheap event.
They are looked at on a schedule, so a jump that starts and lands between two samples is not counted.

```hocon
objectives {
  sampling                   = true   # false: no task runs per player
  sample-interval-ticks      = 10
  placeholder-interval-ticks = 40
  look-distance              = 5
}
```

A server that writes none of these types sets `sampling = false`.

## Anti-glitch

Three separate levels stop a player finishing a mining quest by placing a block and breaking it again.

| Level | Where | Default | What it does |
|---|---|---|---|
| One | `reverse = true` on a task | per task | Takes a count back when an event undoes what the task counted, such as a block put back after it was broken. |
| Two | `anti-glitch.session` | on | Remembers every block a player placed and counts nothing for breaking it. Memory only, so it does not survive a restart. `anti-glitch.remembered-blocks` caps it, oldest first. |
| Three | `anti-glitch.coreprotect` | off | Asks CoreProtect about a block the memory does not know, off the thread that owns the block. `anti-glitch.history-days` is how far back; an older block reads as natural. |

Level three is off because it names a plugin the server may not run. Turned on without CoreProtect, the console
says so once and the other levels keep working.

`anti-glitch.bypass-permission` (default `uxmquests.bypass`) turns every level off for the player who holds it,
and the spawn-reason filter with it, so staff can test a quest. Leave it empty to give nobody a bypass.
