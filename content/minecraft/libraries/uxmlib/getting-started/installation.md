---
title: Installation
order: 11
description: Coordinates, Gradle and Maven, the BOM, and shading versus the standalone jar.
icon: download
---

## Requirements

| | |
|---|---|
| Server | Paper 1.21 or newer (developed against 1.21.11). Folia-ready |
| Java | 21 |
| Build | Gradle or Maven — the modules are plain Maven artifacts |

Adventure, MiniMessage and Brigadier come from Paper at runtime. uxmLib compiles against them and
never ships them.

## Coordinates

Artifacts are published through JitPack under the group `com.github.UXPLIMA.uxmLib`, with the git tag
as the version.

<Callout type="warning" title="The group carries the repository name">

There is no `com.github.UXPLIMA:uxmLib` artifact. The group is
`com.github.UXPLIMA.uxmLib` — repository name after a dot — and the coordinate always ends in a
module name.

</Callout>

## Gradle (Kotlin DSL)

```kotlin
repositories {
    mavenCentral()
    maven("https://repo.papermc.io/repository/maven-public/")
    maven("https://jitpack.io")
}

dependencies {
    implementation("com.github.UXPLIMA.uxmLib:uxmlib-gui:VERSION")
    implementation("com.github.UXPLIMA.uxmLib:uxmlib-item:VERSION")
    implementation("com.github.UXPLIMA.uxmLib:uxmlib-command:VERSION")
}
```

Paper's repository is needed as well as JitPack, because the modules compile against the Paper API.

## Gradle (Groovy DSL)

```groovy
repositories {
    mavenCentral()
    maven { url 'https://repo.papermc.io/repository/maven-public/' }
    maven { url 'https://jitpack.io' }
}

dependencies {
    implementation 'com.github.UXPLIMA.uxmLib:uxmlib-gui:VERSION'
    implementation 'com.github.UXPLIMA.uxmLib:uxmlib-item:VERSION'
}
```

## Maven

```xml
<repositories>
  <repository>
    <id>papermc</id>
    <url>https://repo.papermc.io/repository/maven-public/</url>
  </repository>
  <repository>
    <id>jitpack.io</id>
    <url>https://jitpack.io</url>
  </repository>
</repositories>

<dependency>
  <groupId>com.github.UXPLIMA.uxmLib</groupId>
  <artifactId>uxmlib-gui</artifactId>
  <version>VERSION</version>
</dependency>
```

## The BOM

Importing `uxmlib-bom` as a platform lets you list modules without repeating the version:

```kotlin
dependencies {
    implementation(platform("com.github.UXPLIMA.uxmLib:uxmlib-bom:VERSION"))

    implementation("com.github.UXPLIMA.uxmLib:uxmlib-gui")
    implementation("com.github.UXPLIMA.uxmLib:uxmlib-item")
    implementation("com.github.UXPLIMA.uxmLib:uxmlib-storage")
}
```

Use it once you depend on more than two modules. Mismatched versions between `gui` and `item`, which
`gui` depends on, are the kind of problem the BOM exists to make impossible.

## Everything at once

`uxmlib-all` depends on every module:

```kotlin
implementation("com.github.UXPLIMA.uxmLib:uxmlib-all:VERSION")
```

Convenient while exploring; prefer naming the modules you actually use once the plugin settles,
because a shaded jar carries whatever you declared.

## The standalone jar

Rather than shading, drop the aggregate `uxmlib-all` jar into `plugins/` and depend on it as a
normal Paper plugin:

```yaml
# paper-plugin.yml
depend:
  - uxmlib
```

The standalone jar registers only the handful of listeners whose behaviour is driven by item
persistent data; everything else is consumed as an API.

| | Shading | Standalone jar |
|---|---|---|
| Server setup | None | One extra plugin to install and update |
| Jar size | Larger, per plugin | Small |
| Version conflicts | Impossible, if you relocate | One version for every plugin |
| Updating | Rebuild each plugin | Replace one jar |

Shade when you distribute plugins to servers you do not control. Use the standalone jar on your own
network, where one shared, current copy is simpler than rebuilding five plugins to pick up a fix.

## Building from source

Requires a JDK 21 toolchain; Gradle provisions one through the Foojay resolver if needed.

```bash
./gradlew build                   # compile, format check, static analysis, tests
./gradlew spotlessApply           # auto-format before checking
./gradlew :uxmlib-all:shadowJar   # the standalone plugin jar
./gradlew publishToMavenLocal     # install every module into ~/.m2
```
