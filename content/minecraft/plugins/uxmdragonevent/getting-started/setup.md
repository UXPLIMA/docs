---
title: Setting up the event
order: 12
description: Setting the spawn points, building the portal, and testing before scheduling.
icon: map-pin
---

An event cannot start until the plugin knows where players come back to and where the fight happens.
Both are set with commands and saved to `locations.yml`.

## 1. The lobby spawn

Stand where players should return to when the event ends or they leave:

```
/dragon setlobbyspawn
```

This must not be inside the event world — the plugin refuses and tells you so.

## 2. The event spawn

How this works depends on `custom-world.type`.

**With a vanilla end world** (`custom-world.enabled: false`), the plugin uses the end's own spawn and
`settings.spawn-type` decides where players land:

```yaml
settings:
  spawn-type: PLATFORM   # or SURFACE
```

| Value | Where players arrive |
|---|---|
| `PLATFORM` | The obsidian platform near the end spawn, underground — they dig up |
| `SURFACE` | On the surface near the end spawn |

`PLATFORM` is the vanilla arrival experience and gives everyone the same slow start. `SURFACE` puts
them straight into the fight.

**With a custom world**, set both points by hand:

```
/dragon setcustomspawn     # where players arrive
/dragon setdragonspawn     # where the dragon spawns
```

## 3. Save

```
/dragon save
```

Writes the locations. Run it before restarting — spawn points held only in memory are lost.

Check what the plugin thinks it has:

```
/dragon spawndebug
```

## 4. The portal

Players normally join by jumping into an end portal. Two ways to set that up:

### A WorldGuard region

```yaml
settings:
  worldguard-region-name: dragonportal
```

Make a region of that name around the portal at spawn. A player entering an end portal inside it
joins the event.

### End portals by world

```yaml
settings:
  end-portals:
    all-end-portals-leads-to-event: false
    include-all-worlds: false
    list-type: WHITELIST
    world-list:
      - "world_the_end"
    accepted-portal-types:
      - END_PORTAL
```

With `all-end-portals-leads-to-event: true`, every end portal in the listed worlds joins the event
instead of going to the end. `include-all-worlds: true` ignores the list and applies everywhere.

`accepted-portal-types` takes `END_PORTAL`, `NETHER_PORTAL` and `END_GATEWAY`.

<Callout type="note" title="allow-end must be true">

End portals only fire if `allow-end` is enabled in `bukkit.yml`. With it off, the portal block never
triggers and nobody can join this way.

</Callout>

### Or a command

```yaml
settings:
  join-command:
    enabled: false
    require-permission: true
```

`/dragon join` is disabled by default — the portal is the intended door. Enable it if you have no
portal, and leave `require-permission: true` if it should be a perk.

## 5. Test it

```
/dragon start
```

Starts an event immediately, regardless of the schedule. Join it, check the boss bar, the dragon's
health and the world border, then:

```
/dragon stop
```

Run the whole cycle once before you let the schedule do it unattended. An event that starts with no
spawn point set announces itself to the whole server and then refuses everyone who tries to join.

## 6. The schedule

```yaml
auto-start:
  enabled: true
  max-per-month: 100
  timezone: 'GMT+3'
  days:
    Monday:
      times: "01:15, 12:05, 20:00"
```

See [Scheduling](../features/scheduling.md).
