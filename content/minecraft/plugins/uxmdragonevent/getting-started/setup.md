---
title: Setting up the event
order: 12
description: The lobby spawn, the entrance players join through, and a test run before the schedule takes over.
icon: map-pin
---

An event needs two things from you: a lobby spawn to send players back to, and an entrance. The
event world, its spawn and the dragon's spawn are handled by the plugin unless you use a
[custom world](custom-world.md).

## 1. The lobby spawn

Stand where players should land when they leave or the event ends, and run:

```
/dragon setlobbyspawn
```

It cannot be in the event world. Without a lobby spawn no event starts: staff online are told
`spawn-not-set` instead.

Do not put the lobby spawn inside the entrance region described below. A player sent back to the
lobby would walk straight into the entrance again.

## 2. Where players arrive

With the default world settings the plugin builds a fresh end world before every event, and
`settings.spawn-type` decides where players land in it:

```yaml
settings:
  spawn-type: PLATFORM
```

| Value | Where players arrive |
|---|---|
| `PLATFORM` | A 5 by 5 obsidian platform inside the main island, at x 0, y 50, z -30. Players dig their way up |
| `SURFACE` | Next to the world's spawn point, on the surface |

`PLATFORM` gives everyone the same slow start. `SURFACE` puts them straight into the fight. The
dragon appears 25 blocks above the world's spawn point.

With a custom world, you set both points yourself: see [A custom event world](custom-world.md).

## 3. The entrance

Players join in one of two ways, and `settings.end-portals.all-end-portals-leads-to-event` decides
which. The two do not work together.

### A WorldGuard region (the default)

```yaml
settings:
  lobby-world-name: world
  worldguard-region-name: dragonportal
  end-portals:
    all-end-portals-leads-to-event: false
```

Build an entrance in `lobby-world-name` and define a WorldGuard region named `dragonportal` around
it. A player who walks into the region joins the event. The entrance can be built from anything:
portal blocks, water, lava, or a doorway; only the region counts.

While no event is running, a player who walks in is told `not-active` and sent to the lobby spawn.

### End portals anywhere

```yaml
settings:
  end-portals:
    all-end-portals-leads-to-event: true
    include-all-worlds: false
    list-type: WHITELIST
    world-list:
      - "world"
    portal-behavior-when-event-is-inactive: REGION_CHECK
    accepted-portal-types:
      - END_PORTAL
```

With this on, a portal in the listed worlds takes players to the event instead of where it normally
goes. `world-list` names the worlds the portals are in; `include-all-worlds: true` ignores the list.
`accepted-portal-types` takes `END_PORTAL`, `NETHER_PORTAL` and `END_GATEWAY`.

`portal-behavior-when-event-is-inactive` decides what a portal does while no event is running:

| Value | While no event is running |
|---|---|
| `REGION_CHECK` | A portal inside the WorldGuard region sends the player to the lobby spawn; every other portal works as usual |
| `TELEPORT_LOBBY` | Every listed portal sends the player to the lobby spawn |
| `TELEPORT_BACK` | The player stays where they are and is told `not-active` |
| `NOTHING` | Portals work as usual |

<Callout type="note" title="The server must allow the portal's dimension">

An end portal only fires with `allow-end: true` in `bukkit.yml`, and a nether portal only with
`allow-nether: true` in `server.properties`. With either off, that portal never triggers and nobody
can join through it.

</Callout>

### A command, as well

```yaml
settings:
  join-command:
    enabled: false
    require-permission: true
```

`/dragon join` is off by default. It works next to either entrance; see [Joining](../features/joining.md).

## 4. Save

```
/dragon save
```

Spawn points are kept in memory until they are written to `locations.yml`. The plugin also writes
them every 10 minutes, when an event ends and on shutdown, but saving now means a crash cannot lose
them.

## 5. Test it

```
/dragon start
```

Starts an event now, whatever the schedule says. The world is rebuilt first, so you are told
`starting` and the event begins a few seconds later. Join it, check the boss bar, the dragon's health
and the world border, then:

```
/dragon stop
```

Run the whole cycle once before you let the schedule do it unattended.

## 6. The schedule

```yaml
auto-start:
  enabled: true
  timezone: 'Europe/Istanbul'
  days:
    Monday:
      times: "01:15, 12:05, 20:00"
```

See [Scheduling](../features/scheduling.md).
