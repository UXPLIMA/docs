---
title: Developer
order: 180
description: The three artifacts, their coordinates, and which one your plugin needs.
icon: code
---

uxmClaims 4.0 publishes three libraries under the group `com.uxplima.uxmclaims`. A plugin that only
reads lands or listens to events needs the API. A plugin that adds its own module to uxmClaims needs
the SDK, which pulls in the API.

## Which artifact

| Artifact | Packages | Use it when |
|---|---|---|
| `uxmclaims-api` | `com.uxplima.uxmclaims.api` | You read lands, towns and the rest, check protection, write through the services, or subscribe to events on the uxmClaims event bus |
| `uxmclaims-api-bukkit` | `com.uxplima.uxmclaims.bukkit.event` | You want plain Bukkit `@EventHandler` listeners (or Skript) instead of the uxmClaims event bus |
| `uxmclaims-sdk` | `com.uxplima.uxmclaims.sdk` | You write a module: config files, commands, menus, tables, flags and role permissions that uxmClaims loads and unloads for you |
| `com.uxplima.claim:bukkit-api` | `com.uxplima.claim` | Only for plugins built against 3.x; see [Legacy API](legacy-api.md) |

`uxmclaims-api-bukkit` and `uxmclaims-sdk` both depend on `uxmclaims-api`, so you never declare the API
twice. The Spigot API is not included: your build already has it.

## Gradle and Maven

<Tabs>
<Tab label="Gradle">

```groovy
repositories {
    maven { url = 'https://repo.uxplima.com/repository/maven-releases/' }
}

dependencies {
    compileOnly 'com.uxplima.uxmclaims:uxmclaims-api:4.0.0'
    // Only for Bukkit listeners on uxmClaims events:
    compileOnly 'com.uxplima.uxmclaims:uxmclaims-api-bukkit:4.0.0'
    // Only for a module:
    compileOnly 'com.uxplima.uxmclaims:uxmclaims-sdk:4.0.0'
}
```

</Tab>
<Tab label="Maven">

```xml
<repositories>
    <repository>
        <id>uxplima-releases</id>
        <url>https://repo.uxplima.com/repository/maven-releases/</url>
    </repository>
</repositories>

<dependencies>
    <dependency>
        <groupId>com.uxplima.uxmclaims</groupId>
        <artifactId>uxmclaims-api</artifactId>
        <version>4.0.0</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
```

</Tab>
</Tabs>

Versions ending in `-SNAPSHOT` are in `https://repo.uxplima.com/repository/maven-snapshots/`. Each artifact ships with its sources and javadoc jars.

<Callout type="warning" title="Never shade uxmClaims">

Use `compileOnly` or `provided`. A shaded copy is a second set of classes the running plugin does not
know: `UxmClaimsApi.get()` then throws, and your listeners never fire.

</Callout>

## Requirements

| Requirement | Value |
|---|---|
| Java | 21 (the libraries are compiled with `--release 21`) |
| Server | Spigot 1.19.4 or newer, or a fork such as Paper, Purpur or Folia |
| `plugin.yml` | `softdepend: [uxmClaims]` when your plugin works without it, `depend: [uxmClaims]` when it does not or when it registers a module |

## Getting the API

```java
import com.uxplima.uxmclaims.api.UxmClaimsApi;

// In onEnable, after uxmClaims has enabled:
UxmClaimsApi.find().ifPresentOrElse(
        api -> getLogger().info("Hooked into uxmClaims " + api.version()),
        () -> getLogger().info("uxmClaims is not running; claim features are off."));
```

`UxmClaimsApi.get()` throws `IllegalStateException` until uxmClaims has finished enabling, and again after
it disables. The same object is registered in Bukkit's `ServicesManager` under `UxmClaimsApi.class`.

## Pages in this section

| Page | Covers |
|---|---|
| [The API](api.md) | `UxmClaimsApi`, the services of each module, writes, `Actor`, views and futures |
| [Events](events.md) | Every API event, listening, cancelling, threads and the Bukkit mirrors |
| [Writing a module](sdk.md) | `UxmModule`, the descriptor, the contexts, config, storage and registries |
| [Commands](commands.md) | The command annotations and `commands.yml` labels |
| [Menus](menus.md) | Menu actions, list menus, hub buttons, dialogs and Bedrock forms |
| [Legacy API](legacy-api.md) | Moving a 3.x plugin to 4.0, and what the 3.x bridge still does |

Related: [Modules](../modules/index.md), [Upgrading from 3.x](../upgrading/from-3x.md)
