---
title: Worlds and the border
order: 34
description: The arena, vanilla or custom, the world border and end crystals.
icon: globe
---

## The three worlds

```yaml
settings:
  lobby-world-name: world
  event-world-name: 'dragonevent_end'
  worldguard-region-name: dragonportal
```

`lobby-world-name` holds the spawn players return to. `event-world-name` is the arena — managed and
reset by the plugin. `worldguard-region-name` is the region containing the entry portal.

## Vanilla or custom

```yaml
custom-world:
  enabled: false
  type: CUSTOM
  folder-name: ""
```

| Mode | Behaviour |
|---|---|
| `custom-world.enabled: false` | Uses the world named in `event-world-name` |
| `type: VANILLA` | Same — your existing end world is the arena |
| `type: CUSTOM` | Uses a world folder placed inside the plugin's folder, named in `folder-name` |

A custom world is the better option for anything you have built. Drop the world folder into
`plugins/uxmDragonEvent/`, name it in `folder-name`, and set the two spawn points with
`/dragon setcustomspawn` and `/dragon setdragonspawn`.

With a vanilla end, spawn placement is controlled by `settings.spawn-type` instead — `PLATFORM` or
`SURFACE`.

<Callout type="warning" title="The event world gets reset">

Whichever mode you use, the plugin resets `event-world-name` between events. Never point it at a
world players build in, and never at your live end. A custom world folder is copied fresh each time,
which is exactly what you want for an arena.

</Callout>

## The world border

```yaml
world-border:
  enabled: true
  size: 1000
  center-x: 0
  center-z: -30
```

`size` is the diameter in blocks, not the radius. The default is a 1000-block square centred just
south of the end spawn — which is where the vanilla end island sits.

The border is what keeps the fight in one place. Without it, a dragon in the vanilla end can drift
toward the outer islands and take the event with it.

Set `center-x` and `center-z` to your arena's centre if you are using a custom world, and size it to
the arena rather than to the dragon's flight range — a border that is too generous is the same as no
border.

## End crystals

```yaml
settings:
  delete-end-crystals: true
```

Removes every end crystal from the event world. On 1.20 and above.

Keep this on for a vanilla end: the crystals heal the dragon, and a fight where players have to break
ten pillars first is a much longer fight than your `dragon-event-time` accounts for. If you want that
fight, turn it off and raise the timer.

`end-rules.disableCrystalDamage` and `disableCrystalDestroy` control the rest of the crystal
behaviour.

## Structures

```yaml
end-rules:
  generateStructures: false
```

With this off, end cities and other structures do not generate in the event world. Worth keeping off:
an arena is not a place for loot, and generating structures for a world that gets reset every event
is work the server does not need to do.

## Resetting

```
/dragon reset
```

Rebuilds the event world now. Normally handled automatically between events; run it by hand if an
event ended badly and the arena is in a state you do not want the next one to start in.
