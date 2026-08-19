---
title: uxmlib-integration
order: 26
description: Soft-dependency hooks behind present-guards, native Display holograms, transient toasts, Discord webhooks and an online-data lifecycle manager.
icon: plug
---

Everything that touches something outside your own plugin: another plugin, the Discord API, or the
server's own entity and advancement systems.

One rule runs through the whole module. **Every third-party symbol is touched only past a
plugin-present guard**, so a server without the soft dependency loads cleanly rather than throwing
`NoClassDefFoundError` at enable.

| Page | Covers |
|---|---|
| [Hooks](hooks.md) | Economy, permissions, placeholders, regions, and the deferred hook registry |
| [Holograms](holograms.md) | Native `Display` holograms, appearance, visibility pools, animation |
| [Hologram widgets](hologram-widgets.md) | Paged, switchable and leaderboard holograms, per-viewer content |
| [Toasts](toasts.md) | Transient advancement popups that leave nothing behind |
| [Discord webhooks](discord.md) | Embeds and messages with no bot and no JDA |
| [Online data](online-data.md) | Load on join, flush periodically, save on quit |

## Why guards rather than plugin.yml depends

A `softdepend` in `plugin.yml` controls load order. It does not stop the JVM linking a class you
referenced when the plugin providing it is absent. The guard is what does that:

```java
if (Hooks.isPresent("PlaceholderAPI")) {
    // only now is it safe to reference a PlaceholderAPI type
}
```

Each hook here already does this internally, which is why they return `Optional` rather than throwing.
