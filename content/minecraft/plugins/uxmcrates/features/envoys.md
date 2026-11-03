---
title: Envoys
order: 306
description: Crates that fall across an area on a schedule, how they are collected, and what happens to the ground.
icon: package-open
---

An envoy drops ordinary crates across an area and takes them away again after a while. `envoys/<id>.conf` names the
crate every dropped block is, so everything that crate carries still holds: its key, cost, cooldown, permission,
animation and rewards.

```hocon
name  = "@content.envoys.supply-drop.name"
crate = "common"
drops = 8
lifetime-seconds = 600
area { world = "world", centre-x = 0, centre-z = 0, radius = 200 }
schedule { times = ["13:00", "19:00"] }
on-start   = [ "[broadcast] @envoy.drop.falling", "[sound] BLOCK_BEACON_ACTIVATE 0.7 1.0", "[broadcast] {delay=200} @envoy.drop.half" ]
on-collect = [ "[message] @envoy.drop.reached", "[sound] ENTITY_PLAYER_LEVELUP 0.6 1.2" ]
on-end     = [ "[broadcast] @envoy.drop.gone" ]
```

## Where they land

| `area` | Meaning |
|---|---|
| A circle | `world`, `centre-x`, `centre-z` and `radius`: `drops` crates spread evenly across it |
| `places = ["world,x,y,z", ...]` | Fixed spots, taken as written. Wins over the circle |

A point of the circle carries no height: the crate goes on top of the highest block of that column, kept below the
world's ceiling. A dropped crate is the crate's own block material, or a chest for a `menu` crate. An envoy whose crate
or world does not exist does not start.

## When it runs

| `schedule` | Meaning |
|---|---|
| `times = ["13:00", "19:00"]` | At those times of day |
| `every-seconds = 3600` | On a fixed interval |
| neither | Only when started by hand |

Times win over an interval.

## Collecting

A player opens a dropped crate as they would any placed crate of that id, with its key if it takes one. The first to
open it runs `on-collect`, and the crate is gone. A refused opening collects nothing: a player without a key cannot
make the crate disappear. A crate nobody reaches is taken away after `lifetime-seconds`. When
every crate is collected or the time runs out, `on-end` runs.

## The ground is put back

The block that was in a spot before a crate landed is remembered and put back when the crate is collected, runs out
of time, the envoy is stopped, or the server stops.

## Commands

| Command | Does |
|---|---|
| `/uxmcrates envoy start <envoy>` | Runs it now; `envoy.already-running` if it runs |
| `/uxmcrates envoy stop <envoy>` | Ends it and puts every block back; `envoy.not-running` if it does not |
| `/uxmcrates envoy list` | Every envoy, `running` or `waiting`, and its next run, or `started by hand` |

All three need `uxmcrates.admin.envoy`.

Related: [Envoy files](../config/envoy-files.md), [Crates](crates.md)
