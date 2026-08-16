---
title: Adding Dependency
order: 1620
description: The repository and coordinate to compile against, for Maven and Gradle.
---

The developer API is an ordinary Maven artifact. Add one repository and one coordinate, and your IDE has
the event classes, the front door and the value types with their javadoc.

## The coordinate

<Tabs>
<Tab label="Gradle (Kotlin)">


```kotlin
repositories {
    maven("https://raw.githubusercontent.com/UXPLIMA/uxmEssentials/maven")
}

dependencies {
    compileOnly("com.uxplima.uxmessentials:uxmessentials-bukkit-api:0.7.0")
}
```

</Tab>
<Tab label="Gradle (Groovy)">


```groovy
repositories {
    maven { url 'https://raw.githubusercontent.com/UXPLIMA/uxmEssentials/maven' }
}

dependencies {
    compileOnly 'com.uxplima.uxmessentials:uxmessentials-bukkit-api:0.7.0'
}
```

</Tab>
<Tab label="Maven">


```xml
<repositories>
    <repository>
        <id>uxplima</id>
        <url>https://raw.githubusercontent.com/UXPLIMA/uxmEssentials/maven</url>
    </repository>
</repositories>

<dependencies>
    <dependency>
        <groupId>com.uxplima.uxmessentials</groupId>
        <artifactId>uxmessentials-bukkit-api</artifactId>
        <version>0.7.0</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
```

</Tab>
</Tabs>

`0.6.0` is the first release that carries the API, and `0.7.0` is the current one. Build against the version you target: `/uxmess version` prints
what a server is running, and `UxmEssentialsApi.version()` gives you the same string at runtime.

**Which versions are in the repository.** The repository carries the API from the first release that shipped it onward. Older versions of uxmEssentials
have no artifact and no API classes inside the plugin jar, so a coordinate that resolves is also a promise that
the classes are there at runtime.

`compileOnly` (Maven: `provided`) is the right scope, and the only correct one. The classes live inside the
uxmEssentials plugin jar on the server; shading them into your own jar would give you a second copy of every event
class and Bukkit would then deliver events your listener never sees.

## What the one coordinate gives you

`uxmessentials-bukkit-api` depends on `uxmessentials-api`, so a single line brings both:

| Artifact | Contains | Depends on |
|---|---|---|
| `uxmessentials-bukkit-api` | The front door (`UxmEssentialsApi`), every Bukkit event, the menu surface | Paper, `uxmessentials-api` |
| `uxmessentials-api` | Pure value types with no Bukkit in them: `UxmLocation`, `UxmMoney`, `UxmTeleportKind` and the rest | nothing |

The split exists so a proxy-side, web-side or test-side piece of your project can speak the same vocabulary without
dragging a server API onto its classpath. If you are writing a Paper plugin, take `uxmessentials-bukkit-api` and
ignore the split.

Sources jars are published alongside both, so your IDE shows the javadoc without a separate download.

## Your paper-plugin.yml

Nothing is required. Declaring a dependency on uxmEssentials is a choice, not a rule:

```yaml
name: MyAddon
main: com.example.MyAddon
version: '1.0.0'
api-version: '1.21'
```

[`UxmEssentialsApi.whenReady`](overview.md#the-front-door) handles either load order for you, and Bukkit delivers
events from a plugin that loaded later just the same. Add a soft dependency only if you have your own reason to
influence load order:

```yaml
dependencies:
  server:
    uxmEssentials:
      load: BEFORE
      required: false
```

<Callout type="warning" title="Never make it required">

`required: true` means your plugin refuses to load on a server without uxmEssentials. Since every entry point in
this API is null-safe or callback-based, there is nothing to gain from it and a support ticket to lose.

</Callout>

## A complete example

The [sample consumer](https://github.com/UXPLIMA/uxmEssentials/tree/main/sample-consumer) in the repository is a
small, compiling plugin that uses the front door, one veto listener and two notification listeners. It is built in CI
against the published artifacts on every commit, so what it shows is what currently works.

## Next steps

- [Overview](overview.md) for the front door and the shape of the API
- [Events](events.md) for everything you can listen to and everything you can cancel
- [Menu API](menu-api.md) to extend the GUI engine rather than only observe it
