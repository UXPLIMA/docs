---
title: Overview
order: 860
description: uxmSkyblock exposes a clean API so other plugins and modules can read
  and modify islands. The core is published through JitPack.
---

---

## Adding the Dependency

The core runs as its own plugin on the server, so you compile against it but don't
bundle it — use `compileOnly` / `provided`.

<Tabs>
<Tab label="Gradle">

```gradle
repositories {
    maven { url 'https://jitpack.io' }
}

dependencies {
    // A release tag, or 'main-SNAPSHOT' for the latest commit on main.
    compileOnly 'com.github.UXPLIMA:uxmSkyblock:0.4.0'
}
```

</Tab>
<Tab label="Maven">

```xml
<repository>
    <id>jitpack.io</id>
    <url>https://jitpack.io</url>
</repository>

<dependency>
    <groupId>com.github.UXPLIMA</groupId>
    <artifactId>uxmSkyblock</artifactId>
    <version>0.4.0</version>
    <scope>provided</scope>
</dependency>
```

</Tab>
</Tabs>

---

## What the API Gives You

| Type | Purpose |
|------|---------|
| `UxmSkyblockPlugin` | Entry point: `getIslandManager()`, `getRoleManager()`, `getEconomy()`, `getTopService()`, `getWarpService()`, ... |
| `Island` / `IslandManager` | Islands, members, roles, points, bank, warps |
| `RoleData` / `RoleResolver` | Built-in and per-island custom roles |
| `IslandCreateEvent` / `IslandDeleteEvent` | Bukkit events to listen for |
| `UxmSkyblockModule` + `ModuleContext` | Write a drop-in module |

---

## Getting the Plugin Instance

```java
UxmSkyblockPlugin core =
    (UxmSkyblockPlugin) Bukkit.getPluginManager().getPlugin("uxmSkyblock");
```

If you're writing a **module**, you instead receive the core through your
`ModuleContext` in `onLoad` — no `getPlugin` lookup needed. See
[Writing a Module](modules.md).

---

## Example — Read and Modify an Island

```java
Island island = core.getIslandManager().getByMember(player.getUniqueId());
if (island != null) {
    island.depositBank(1000);
    core.getIslandManager().saveAsync(island);
}
```

`getByMember` finds the island a player belongs to (as owner or member); there's
also `getByOwner` and `getIslandAt(Location)` (an O(1) grid lookup).

<Callout type="warning" title="Save your changes">

Mutating an `Island` only changes it in memory. Call
`getIslandManager().saveAsync(island)` to persist (and, on a network, broadcast
the change over the proxy).

</Callout>

---

## Next

- [Events](events.md) — react to island creation/deletion
- [Writing a Module](modules.md) — the drop-in add-on system
