---
title: The packet layer
order: 31
description: The clean-room packet foundation, why it exists, and why it is unstable.
icon: flask-conical
---

`uxmlib-npc`, `uxmlib-packet` and `uxmlib-nametags` are an in-progress, clean-room packet foundation.

<Callout type="warning" title="Experimental: unstable APIs">

These three modules are a preview. Names and signatures may change without notice until they
graduate, and parts are still landing. The stable toolkit does not depend on them, so you can ignore
them entirely.

</Callout>

## Why they exist

The public Paper API cannot do per-viewer versions of things: a different nametag colour, tab-list
row or hologram for each player looking at the same object. Everything else in uxmLib is built on the
public API precisely so it keeps working across point releases; these three are the deliberate
exception, for the cases that have no other route.

PacketEvents is the off-the-shelf answer and is GPL, which would make uxmLib GPL. None of it is
borrowed: the Netty plumbing is re-implemented for Paper 1.21+, and the unavoidable NMS is
quarantined in single, named classes behind pure ports built against the Mojang-mapped dev bundle.

## uxmlib-npc

The pipeline foundation, and no NPC yet despite the name:

- channel resolution for a player
- idempotent inject and eject, so double-injection is not a state to get into
- a self-healing reorder watchdog: another plugin inserting itself into the pipeline does not
  silently disable yours
- a fail-open listener seam: a listener that throws does not drop the connection

## uxmlib-packet

Shared Mojang-mapped helpers: Adventure to vanilla component conversion, packet bundling, the
stream-codec buffer trick, guarded reflection, entity-id allocation, plus per-viewer ports for tab
list, NPCs and text displays built on them.

## uxmlib-nametags

A per-viewer nametag renderer over scoreboard-team and metadata packets, **without touching the
server-side scoreboard**.

```java
NametagRenderer renderer = ...;
```

That distinction is the point. The usual approach writes to the server scoreboard, which means two
plugins doing it fight, and a player's team membership becomes global state. This does not touch it,
so per-viewer prefixes, colours and visibility coexist with whatever else the server runs.

`Appearance`, `Billboard`, `Alignment` and `PerViewerText` describe what a viewer sees;
`LineOfSight` and `BlockLineOfSight` handle occlusion.

## Using them anyway

If you do:

- Pin an exact version. "Unstable" means a minor release may change a signature.
- Expect to update on Minecraft point releases. NMS is quarantined, not eliminated.
- Keep them behind an interface of your own, so replacing them later is one class.

For anything the public API can do (a hologram everyone sees the same, a sidebar, a boss bar) use
[`uxmlib-integration`](integration.md) and [`uxmlib-hud`](hud.md) instead. They are stable, and they
survive server updates without you doing anything.
