---
title: Nametags
order: 313
description: "A per-viewer nametag renderer: different prefixes, colours and visibility per viewer, without touching the server-side scoreboard."
icon: tag
---

Paper can give a player one nametag. `uxmlib-nametags` gives each viewer their own, built from
scratch over the packet layer and never touching the server-side scoreboard.

## Showing one

```java
NametagRenderer renderer = new NametagRenderer(packets, scheduler);
NametagRenderer withSight = new NametagRenderer(packets, scheduler, new BlockLineOfSight());

NametagHandle handle = renderer.show(
        target,
        Appearance.defaults(),
        viewers,
        viewer -> Text.mini("<gray>[" + rankOf(viewer, target) + "] " + target.getName()));

handle.update();
handle.remove();
```

The viewer set may be a `Supplier<Set<UUID>>` instead of a fixed set, for a nametag whose audience
changes: a team-only tag, a tag visible to staff.

`PerViewerText` is the per-viewer content function. It is called per viewer, which is the whole point:
the same target can read as a friend to one player and as an enemy to another.

## Appearance

`Appearance` is a record with `translation`, `scale`, a `Billboard` and an `Alignment`, plus the
usual display settings. `Appearance.defaults()` is the sensible starting point.

## Line of sight

```java
LineOfSight sight = new BlockLineOfSight();
```

Hides the tag from a viewer whose view is obstructed, which is what makes a per-viewer tag feel like
part of the world rather than an overlay. `LineOfSight` is an interface, so a different rule is a
small class.

## Refresh

The renderer re-evaluates on a period, defaulting to 500 milliseconds
(`NametagRenderer.DEFAULT_REFRESH_PERIOD`). That is the trade-off between a tag that reacts to a rank
change quickly and one that costs nothing when nothing changed.

## What it does not do

It does not touch the server-side scoreboard, so it cannot collide with a scoreboard plugin, and it
does not consume the sixteen-team budget that scoreboard-based nametag plugins fight over.

<Callout type="warning" title="Experimental">

The API may change without notice. If you need a nametag today and can accept one per player rather
than one per viewer, the public scoreboard API through `uxmlib-hud` is the stable choice.

</Callout>
