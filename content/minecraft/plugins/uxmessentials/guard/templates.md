---
title: Templates and World Defaults
order: 1100
description: The six shipped templates line by line, writing your own, and the rules a whole world starts from.
icon: layout-template
---

Contents: [Using a template](#using-a-template) · [The shipped templates](#the-shipped-templates) ·
[Writing your own](#writing-your-own) · [World defaults](#world-defaults)

## Using a template

A template is a named set of rules and behaviours in `modules/guard/config.conf`. Linking a region to one gives
the region everything in it:

```text
/guard template market market
/guard template market clear
/guard tag bulk shops template market
```

The link is live. Edit the template in the config and run `/guard reload`, and every region linked to it
changes. Anything the region sets itself wins over the template for the same action and target (see
[Where rules come from](rules.md#where-rules-come-from)), so a template is a starting point you can override one
line at a time.

## The shipped templates

### spawn

A server spawn: nobody changes anything, doors and buttons work, nobody gets hurt, no monsters.

| Rules | Behaviours |
|---|---|
| `break`, `place`, `use`, `interact`, `damage`, `item.drop`, `item.use.ender-pearl`, `item.use.bucket`, `item.use.lighter`, `hanging`, `vehicle`, `trample`: everyone deny | `invincible = true` |
| `use.door`, `use.gate`, `use.button`, `use.pressure-plate`: everyone allow | `spawn.deny = monster` |
| `world.explosion`, `world.fire`, `world.mob`: everyone deny | |

### lobby

A lobby: like `spawn`, and players keep everything, never starve, see clear weather, and fall back up out of the
void. Nothing spawns at all.

| Rules | Behaviours |
|---|---|
| `break`, `place`, `use`, `interact`, `damage`, `item.drop`, `item.pickup`, `item.use.ender-pearl`, `item.use.bucket`, `item.use.lighter`, `item.use.spawn-egg`, `hanging`, `vehicle`, `trample`: everyone deny | `invincible = true`, `keep-inventory = true`, `keep-exp = true` |
| `use.door`, `use.button`, `use.pressure-plate`: everyone allow | `feed = 20 10`, `weather = clear` |
| `world`: everyone deny (all of nature) | `spawn.deny = *`, `void-teleport = -64 spawn` |

### arena

A fighting area: players may hurt each other, but not escape by pearl or teleport, nor break the arena.

| Rules | Behaviours |
|---|---|
| `break`, `place`, `use.container`, `use.workstation`, `use.bed`, `item.use.ender-pearl`, `item.use.chorus-fruit`, `item.use.bucket`, `item.use.lighter`, `hanging`, `world.explosion`, `world.fire`: everyone deny | `fly = deny` |
| `damage.player`: everyone allow | `commands.deny = home, spawn, tpa, tpaccept, warp, back` |

### market

A shop street: members run their stalls, visitors use doors, buttons and signs.

| Rules | Behaviours |
|---|---|
| `break`, `place`, `use`, `interact`, `hanging`: nonmember deny | `invincible = true` |
| `use.door`, `use.button`, `use.sign`: everyone allow | |
| `damage`, `item.use.lighter`, `world.explosion`, `world.fire`: everyone deny | |

### parkour

A course: no shortcuts by flight, elytra, pearls, fireworks, wind charges, chorus fruit or teleport, and no
effects that help.

| Rules | Behaviours |
|---|---|
| `break`, `place`, `use`, `interact`, `damage`, `item.drop`, `item.use.ender-pearl`, `item.use.firework`, `item.use.wind-charge`, `item.use.chorus-fruit`, `move.teleport-in`, `trample`: everyone deny | `fly = deny`, `glide = deny`, `invincible = true` |
| `use.pressure-plate`, `use.button`: everyone allow | `effects.block = speed, jump_boost, levitation, slow_falling` |

### build

A shared build area: members build, everyone else looks.

| Rules | Behaviours |
|---|---|
| `break`, `place`, `use`, `interact`, `hanging`, `vehicle`, `item.use.bucket`, `item.use.lighter`, `damage.animal`, `damage.armor-stand`, `damage.hanging`: nonmember deny | none |
| `world.explosion`, `world.fire.spread`: everyone deny | |

## Writing your own

Add a block under `templates` in `modules/guard/config.conf`, then `/guard reload`:

```hocon
templates {
  shop-plot {
    rules = [
      "break nonmember deny"
      "place nonmember deny"
      "use.container nonmember deny"
      "world.explosion everyone deny"
    ]
    behaviours = [
      "message.enter = <yellow>{player}, welcome to the shop"
      "commands.deny = sethome"
    ]
  }
}
```

A rule line is `<action> <target> <allow|deny>`; a behaviour line is `<behaviour> = <value>`. A line guard
cannot read is skipped with a warning in the console naming the template, so a typo never stops the module
from starting.

## World defaults

Each world has an invisible region under every other one, filled from `worlds` in the config. It decides only
what no region answered. `default` applies to every world without an entry of its own.

```hocon
worlds {
  default {
    high-frequency = true
    rules = []
    behaviours = []
  }
  world_nether {
    rules = [ "world.fire everyone deny" ]
  }
}
```

| Key | Default | What it does |
|---|---|---|
| `high-frequency` | `true` | `false` skips liquid flow, fire spread, growth, decay, forming and melting in this world even where rules name them, for a world where those rules do not matter |
| `rules` | `[]` | Rules for the whole world, in rule line form |
| `behaviours` | `[]` | Behaviours for the whole world, in behaviour line form |

A `global` region (`/guard define <name> global`) does the same job and can be edited in game; the importer turns
a WorldGuard `__global__` region into one. Events a world has no rule for cost nothing, whichever way the rules
are written.
