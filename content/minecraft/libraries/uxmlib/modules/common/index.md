---
title: uxmlib-common
order: 21
description: "The foundation every other module sits on: a Folia-ready scheduler, MiniMessage text, i18n, typed config, particles and helpers."
icon: layers-2
---

`uxmlib-common` depends on nothing internal, and everything else depends on it. If you take one
module, take this one.

It is not a grab bag. Each part of it exists because a Paper plugin cannot avoid the problem it
solves: you cannot avoid scheduling, you cannot avoid text, you cannot avoid config, and you cannot
avoid the fact that a player might be on Bedrock.

| Page | Covers |
|---|---|
| [Scheduler](scheduler.md) | Paper's four schedulers behind one interface, ticks and the tick clock |
| [Text](text.md) | MiniMessage, placeholders, client capability downgrade, glyph widths |
| [Messages and i18n](messages.md) | Message catalogs, locale fallback, admin-retargetable channels |
| [Configuration](configuration.md) | HOCON config, live properties, migrations, validation, typed records |
| [Particles](particles.md) | Type-safe particle spawning |
| [Utilities](utilities.md) | Durations, numbers, versions, sounds, ReDoS-guarded regex |

## The shape of everything here

Two rules run through the whole module and are worth stating once.

**Constructor injection, no static state.** `new PaperScheduler(plugin)` is built once and passed
around. There is no `getInstance()`, no service locator, and no static mutable state anywhere in the
library. Two plugins on one server each hold their own instances, and a test constructs one with a
fake instead of standing up a server.

**Pure where it can be.** The logic that does not need a server is separated from the logic that
does, so it can be unit-tested without one. `MessageCatalog` resolves a locale with no server.
`RenderDowngrade` rewrites a string with no Adventure. `SemanticVersion` compares with nothing at all.
That split is why the test suite is fast and why the behaviour is predictable.
