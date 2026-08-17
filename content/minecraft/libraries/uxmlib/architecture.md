---
title: Architecture
order: 30
description: The rules uxmLib is written to, and what they mean for a plugin built on it.
icon: blocks
---

The rules uxmLib is written to, and what they mean for a plugin built on it.

## One platform, done well

Paper 1.21+ and Java 21 only. Every API targets the current server, with no reflection machinery for
versions that no longer exist.

A cross-version library pays for that reach in every method — reflection lookups, per-version
branches, and an API shaped by the oldest thing it supports. Dropping that is what lets the item
builder use registry keys directly and the scheduler map onto Folia's four schedulers without an
abstraction over an abstraction.

The cost is real: uxmLib will not run on 1.20.

## No upward dependencies

`common` depends on nothing internal. Everything may depend on `common`. `gui` depends on `item`.
`all` aggregates. `redis` depends on nothing at all.

ArchUnit tests enforce it, so the graph is a guarantee rather than a convention — and taking one
module is genuinely taking one module, not discovering a transitive half of the library.

## Constructor injection, no static state

The only `JavaPlugin` is the thin shell in `uxmlib-all`. Library types are plain objects you
construct and inject.

```java
Scheduler scheduler = new PaperScheduler(plugin);
SidebarManager sidebars = new SidebarManager(Bukkit.getScoreboardManager());
```

No `getInstance()`, no service locator, no static mutable state. Two plugins on the same server each
hold their own instances, and a test constructs one with a fake instead of standing up a server.

## Folia-ready from line one

Nothing schedules through `BukkitScheduler`. The `Scheduler` abstraction maps onto Paper's global,
region, entity and async schedulers, so the same plugin code runs unchanged on Folia.

That only holds if you pick the right one. Folia's threading model is the reason the four exist —
touching a block from the wrong region is the bug the abstraction makes visible rather than
prevents.

## Adventure-native

All text is Adventure components built from MiniMessage. Legacy `§` and `&` codes are deliberately
unsupported.

One representation means text renders the same whichever path it took, and gradients, hover events
and click events are available everywhere rather than in the places someone remembered to use
components.

## Null-safe and statically checked

Every package is JSpecify `@NullMarked`. NullAway and Error Prone run as errors under `-Werror`,
formatting is enforced by Spotless with Palantir Java Format, and ArchUnit guards the module
boundaries.

`@NullMarked` means a reference is non-null unless annotated otherwise — so `Optional` in a return
type is a real signal, and a parameter that accepts null says so.

## Native where it can be

GUIs, holograms, HUD overlays and toasts use the public Paper and Adventure API. No packets, no
per-version NMS — so they keep working across point releases without anyone updating anything.

The [experimental packet modules](modules/experimental.md) are the deliberate exception, for
per-viewer effects the public API cannot express.

## MIT, and clean-room

Nothing is copied from GPL, AGPL or proprietary sources. The Minecraft-facing API is written from
scratch; only neutral infrastructure — HikariCP, Caffeine, Configurate — is taken as a dependency.

Use it anywhere, including in closed-source plugins.

Where a permissively licensed project informed an approach, it is acknowledged in the repository's
README: Triumph GUI and AnvilGUI for menu and anvil-input patterns, Item-NBT-API and Rtag for item
data, Lamp for command annotations, HamsterAPI for the pipeline-injection technique, and
FancyHolograms for the per-viewer text-override approach.

## Testing

JUnit 5, AssertJ, Mockito, MockBukkit on the Paper 1.21 line, jqwik property tests, and ArchUnit.

## Versioning

Semantic versioning. Public API modules aim for stable names and documented seams.

`0.x` releases may still adjust APIs between minor versions while the surface settles, and the
experimental modules may change without notice until they graduate. Pin an exact version and read
the release notes before moving.
