---
title: Shading and relocation
order: 12
description: Relocating the library, and the minimize() setting that breaks config parsing.
icon: box
---

If you shade uxmLib into your own jar, relocate it. Two plugins bundling the same classes at the same
package names is a class-loading problem that shows up as inexplicable `NoSuchMethodError`s at
runtime, and the version that wins is whichever loaded first.

```kotlin
tasks.shadowJar {
    relocate("com.uxplima.uxmlib", "com.yourplugin.libs.uxmlib")
}
```

## minimize() and the ServiceLoader

The config and storage layers find their codecs through the JDK `ServiceLoader`. Shadow's
`minimize()` removes classes it cannot see referenced, and a service implementation is referenced
only by a text file under `META-INF/services` — so minimisation strips exactly the classes that make
config parsing work.

The failure is a missing-serializer error at runtime, long after the build succeeded.

```kotlin
tasks.shadowJar {
    minimize {
        exclude("META-INF/services/**")
        exclude(dependency("org.spongepowered:configurate-.*:.*"))
    }
}
```

<Callout type="tip" title="Test the shaded jar, not the classpath">

Both of these problems only appear in the built artifact. A plugin that runs from your IDE proves
nothing about the jar you ship — load the shaded jar on a real server once before releasing.

</Callout>

## What is safe to minimise

Everything else. uxmLib's own classes are referenced normally and survive minimisation; it is the
Configurate service providers specifically that need the exclusion.

## Dependencies you inherit

Shading uxmLib brings its dependencies with it:

| Module | Brings |
|---|---|
| `uxmlib-common` | Configurate (HOCON) |
| `uxmlib-storage` | HikariCP, Caffeine, the SQLite driver |
| `uxmlib-redis` | Nothing — Lettuce is compile-only |

Adventure, MiniMessage and Brigadier are provided by Paper and are never shipped, whichever route you
take.

`uxmlib-redis` treats Lettuce as a compile-only soft dependency: you add Lettuce yourself if you use
Redis, and a plugin that does not is not carrying a Redis client it never touches.
