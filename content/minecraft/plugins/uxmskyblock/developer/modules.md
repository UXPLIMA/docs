---
title: Writing a Module
order: 880
description: Building a drop-in module jar against the island API.
icon: blocks
---

A **module** is a drop-in jar that extends uxmSkyblock from inside its own
classloader, with full access to the island API. This page shows how to build one.
For the high-level picture, see [Module System](../modules/overview.md).

---

## 1. Project Setup

Compile against the core API (don't bundle it):

```gradle
repositories {
    maven { url 'https://jitpack.io' }
}
dependencies {
    compileOnly 'com.github.UXPLIMA:uxmSkyblock:0.4.0'
}
```

Build a **plain jar** (not a shaded plugin): the core loads it as a module, not a
Bukkit plugin.

---

## 2. `module.yml`

Put this at the **root** of your jar:

```yaml
name: MyModule
main: com.example.mymodule.MyModule
version: 1.0.0
```

| Key | Meaning |
|-----|---------|
| `name` | Module name (used for its data folder) |
| `main` | Fully-qualified class implementing `UxmSkyblockModule` |
| `version` | Your version string |

---

## 3. The Main Class

Implement `UxmSkyblockModule` and its three hooks:

```java
package com.example.mymodule;

import net.cengiz1.uxmskyblock.module.UxmSkyblockModule;
import net.cengiz1.uxmskyblock.module.ModuleContext;

public class MyModule implements UxmSkyblockModule {

    private ModuleContext ctx;

    @Override
    public void onLoad(ModuleContext ctx) {
        this.ctx = ctx;
        // Grab what you need from the core:
        // ctx.getCorePlugin().getIslandManager(), getEconomy(), ...
        // Your own config.yml + data folder are available here too.
    }

    @Override
    public void onEnable() {
        // Register listeners / commands. Auto-removed on disable.
        ctx.registerListener(new MyListener(ctx));
        ctx.registerCommand("mymodule", new MyCommand(ctx));
    }

    @Override
    public void onDisable() {
        // Stop tasks, flush data, etc.
    }
}
```

---

## 4. What `ModuleContext` Provides

| Capability | How |
|------------|-----|
| The core plugin & its managers | `ctx.getCorePlugin()` → `getIslandManager()`, `getEconomy()`, `getRoleManager()`, ... |
| Register a listener (auto-cleanup) | `ctx.registerListener(listener)` |
| Register a command (auto-cleanup) | `ctx.registerCommand(name, executor)` |
| Your own `config.yml` + data folder | separate from the main plugin |

Because you run in the core's classloader, you also share its **database
connection** and **proxy/Redis messaging**: store and sync your data through the
same pipes the core uses.

---

## 5. Persisting & Syncing Data

- Use the shared SQLite/MySQL connection to add your own tables (Chunklock adds a
  `chunklock_*` table in the same database).
- For cross-server sync, reuse the core's proxy classes (`RedisMessenger`,
  `ProxyManager`) on **your own channel**: the same approach Chunklock uses.

---

## 6. Deploy

Drop the jar into:

```
plugins/uxmSkyblock/modules/MyModule-1.0.0.jar
```

Restart (or reload). The core loads your module after its own startup and unloads
it cleanly on shutdown.

---

## Reference Implementation

[Chunklock](../../chunklock/overview.md) is a full, real module built exactly this
way: read its pages to see the pattern end to end.
