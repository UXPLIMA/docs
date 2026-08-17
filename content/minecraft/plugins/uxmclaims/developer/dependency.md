---
title: Adding the dependency
order: 1002
description: The repository, the artifact, and declaring the soft dependency.
icon: package
---

One dependency gets you the API, the facades, the domain model and the events.

## Gradle

```groovy
repositories {
    maven { url = 'https://repo.inovixi.com/repository/maven-releases/' }
    maven { url = 'https://repo.inovixi.com/repository/maven-snapshots/' }
}

dependencies {
    compileOnly 'com.uxplima.claim:bukkit-api:VERSION'
}
```

## Maven

```xml
<repositories>
    <repository>
        <id>inovixi-releases</id>
        <url>https://repo.inovixi.com/repository/maven-releases/</url>
    </repository>
    <repository>
        <id>inovixi-snapshots</id>
        <url>https://repo.inovixi.com/repository/maven-snapshots/</url>
    </repository>
</repositories>

<dependencies>
    <dependency>
        <groupId>com.uxplima.claim</groupId>
        <artifactId>bukkit-api</artifactId>
        <version>VERSION</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
```

`compileOnly` and `provided` are correct: uxmClaims is already on the server, and shading it would
give you a second, disconnected copy of the domain model.

## Declaring the dependency

In your `plugin.yml`:

```yaml
softdepend:
  - uxmClaims
```

`softdepend` if your plugin works without it, `depend` if it does not. Either way your plugin loads
after uxmClaims, which matters because the API throws until uxmClaims has initialised.

## Guarding the call

```java
if (Bukkit.getPluginManager().getPlugin("uxmClaims") == null) {
    getLogger().info("uxmClaims not found, claim integration disabled.");
    return;
}

UxmClaimBukkitAPI api = UxmClaimBukkitAPI.getInstance();
```

`getInstance()` throws `IllegalStateException` with *"uxmClaims is not initialized yet!"* when called
too early. With `softdepend` set and the call made in `onEnable`, that will not happen, but a static
initialiser, or a call from `onLoad`, will hit it.

## Requirements

| | |
|---|---|
| Java | 21 |
| Server | 1.19.4 or newer |
| Scope | `compileOnly` / `provided` (never shade) |
