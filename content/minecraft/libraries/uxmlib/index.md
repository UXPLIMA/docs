---
title: uxmLib
order: 10
description: "A modular, Folia-ready toolkit for Paper 1.21+ plugins on Java 21: GUIs, items, commands, config, storage, integrations and HUD."
icon: package
---

uxmLib is the toolkit our plugins are built on, and it is open source under MIT: you can use it in
your own, including closed-source ones.

It bundles the parts every Paper plugin ends up reimplementing: inventory menus, item building,
Brigadier commands, typed configuration, pooled storage, soft-dependency hooks, HUD overlays,
holograms, an update checker and a config-driven condition and action engine.

It targets **Paper 1.21+ on Java 21 only**, deliberately. There are no cross-version reflection
layers to carry, nothing schedules through `BukkitScheduler`, and all text is Adventure components
built from MiniMessage: legacy `§` and `&` colour codes are not supported.

- [Getting Started](getting-started/)
- [Modules](modules/)
- [Architecture](architecture.md)

## Links

| | |
|---|---|
| Source | [github.com/UXPLIMA/uxmLib](https://github.com/UXPLIMA/uxmLib) |
| Artifacts | [JitPack](https://jitpack.io/#UXPLIMA/uxmLib) |
| Licence | MIT |
