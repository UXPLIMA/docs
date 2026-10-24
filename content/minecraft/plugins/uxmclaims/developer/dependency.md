---
title: Adding the dependency
order: 1002
description: The repository, the artifact, and declaring the soft dependency.
icon: package
---

One artifact, `com.uxplima.claim:bukkit-api`, gives you the entry point, the facades, the command
objects, the domain model and the events. The current version is `3.7.0`.

## Gradle and Maven

<Tabs>
<Tab label="Gradle">

```groovy
repositories {
    maven { url = 'https://repo.uxplima.com/repository/maven-releases/' }
    maven { url = 'https://repo.uxplima.com/repository/maven-snapshots/' }
}

dependencies {
    compileOnly 'com.uxplima.claim:bukkit-api:3.7.0'
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
    <repository>
        <id>uxplima-snapshots</id>
        <url>https://repo.uxplima.com/repository/maven-snapshots/</url>
    </repository>
</repositories>

<dependencies>
    <dependency>
        <groupId>com.uxplima.claim</groupId>
        <artifactId>bukkit-api</artifactId>
        <version>3.7.0</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
```

</Tab>
</Tabs>

## What the artifact contains

| Artifact | Packages | Holds |
|---|---|---|
| `bukkit-api` | `com.uxplima.claim.bukkit.api` | `UxmClaimBukkitAPI`, `@ClaimEventHandler`, `BukkitConverter` |
| `uxmclaims-app` (pulled in) | `com.uxplima.claim.app` | `ClaimFacade`, `PlayerFacade`, the command objects, `ClaimCommandOptions`, the ports, the config models |
| `uxmclaims-domain` (pulled in) | `com.uxplima.claim.domain` | `Claim` and its parts, the enums, the events, `ClaimException` |

`bukkit-api` re-exports the other two, so you declare only `bukkit-api`. The Spigot API is not
included: your build already has it.

Use `compileOnly` or `provided`, never shade. uxmClaims is already on the server, and a shaded copy
is a second, disconnected set of classes that the running plugin does not know.

## Declaring the dependency

In your `plugin.yml`:

```yaml
softdepend:
  - uxmClaims
```

Use `softdepend` if your plugin works without it, `depend` if it does not. Either way your plugin
enables after uxmClaims.

## Guarding the call

```java
if (!Bukkit.getPluginManager().isPluginEnabled("uxmClaims")) {
    getLogger().info("uxmClaims not found, claim integration disabled.");
    return;
}

UxmClaimBukkitAPI api = UxmClaimBukkitAPI.getInstance();
```

`getInstance()` throws `IllegalStateException` (`uxmClaims is not initialized yet!`) until uxmClaims
has finished starting. Calling it from a static initialiser or from `onLoad` hits this. So does a
server where uxmClaims disabled itself at startup, for example over a missing licence key or a
database another server holds: `isPluginEnabled` is the check that covers that case.

## Requirements

| Requirement | Value |
|---|---|
| Java | 21 |
| Server | Spigot 1.19.4 or newer, or a fork such as Paper, Purpur or Folia |
| Scope | `compileOnly` / `provided` |

Related: [The API](api.md), [Events](events.md)
