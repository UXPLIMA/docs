---
title: Developer API
order: 295
description: Starting builds, reading jobs, adding a protection plugin, and listening to build events.
icon: code
---

The public API is a separate artifact, `com.uxplima:builders-api`. It holds the service, the domain
types and the events, and depends on nothing but the Paper API.

## Adding it

<Tabs>
<Tab label="Gradle">

```groovy
repositories {
    maven { url = 'https://repo.uxplima.com/repository/maven-releases/' }
}

dependencies {
    compileOnly 'com.uxplima:builders-api:1.0.0'
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
        <groupId>com.uxplima</groupId>
        <artifactId>builders-api</artifactId>
        <version>1.0.0</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
```

</Tab>
</Tabs>

In your `plugin.yml`:

```yaml
softdepend: [ uxmBuilders ]
```

`compileOnly`: uxmBuilders is already on the server, and shading the API gives you a second copy that the
running plugin does not know about.

## The service

```java
UxmBuildersAPI api = Bukkit.getServicesManager().load(UxmBuildersAPI.class);
if (api == null) return;   // uxmBuilders is not installed or did not start
```

| Method | Does |
|---|---|
| `startBuild(Player actor, BuildRequest request)` | Start a build; completes on the main thread with a `Result<BuildJob>` |
| `getActiveJobs(UUID player)` | Running, queued and paused jobs of one player |
| `getJob(String jobId)` | One job by id |
| `cancelJob(String jobId, CancelReason reason)` | Cancel; `false` if refused or unknown |
| `getPackages()`, `getBuilders()` | The configured packages and builders |
| `getSelectedBuilder(Player)`, `getBuilderLevel(Player, String)` | A player's builder |
| `setBuilderLevel(Player, String, int)` | Set a level; saved at once |
| `registerProtectionProvider(ProtectionProvider)` | Add your own land check |

## Starting a build

```java
BuildRequest request = BuildRequest.of(player.getUniqueId(), "oak_house", location, 90, false);
api.startBuild(player, request).thenAccept(result -> {
    if (result.failed()) player.sendMessage("Refused: " + result.messageKey());
});
```

A request runs every check a player's build does. The full constructor sets three bypasses:

| Flag | Skips |
|---|---|
| `bypassProtection` | Land protection and placement rules |
| `bypassCost` | The price |
| `bypassMaterials` | Warehouse materials |

The request's owner must be the `actor`; a request for another player is refused with
`errorOwnerMismatch`. A failed `Result` carries a `messages.yml` key and its placeholders.

## A protection provider

```java
api.registerProtectionProvider(new ProtectionProvider() {
    public String id() { return "myclaims"; }
    public boolean isAvailable() { return true; }
    public int priority() { return 50; }
    public ProtectionResult canBuild(Player player, Cuboid box) {
        return MyClaims.canBuild(player, box) ? ProtectionResult.ALLOWED
                : new ProtectionResult.Denied("MyClaims", "claim name");
    }
});
```

Return `ProtectionResult.NOT_APPLICABLE` where your plugin has nothing in the box, so `PERMISSIVE` mode
asks the others. The `Cuboid` offers `samplePoints(n)` if your plugin answers point by point. A server
owner can still switch your provider off under `protection.providers.<id>`.

## Events

| Event | Fires | Cancellable |
|---|---|---|
| `BuildPreValidateEvent` | Before any check; add your own rules | yes, with a message key |
| `BuildStartEvent` | The job is created, before the charge | yes, with a message key |
| `BuildProgressEvent` | Every 5% | no |
| `BuildBlockPlaceEvent` | Every block, only with `api.fireBlockEvents` | yes, skips the block |
| `BuildPauseEvent` | A pause by a player or a plugin | yes, the job keeps running |
| `BuildBoostEvent` | Before a boost is paid; the price can change | yes |
| `BuildCancelEvent` | Before a cancel; the refund can change | yes, except for safety cancels |
| `BuildCompleteEvent` | The last block is placed | no |
| `BuilderNpcSpawnEvent` | Before the NPC appears; name, skin and mob can change | yes |
| `BuilderUpgradeEvent` | A builder level upgrade; the price can change | yes |

Every event extends `BuildEvent` and fires on the main thread. Safety cancels, when land is lost, and
safety pauses, for an offline owner, an unloaded world or missing materials, cannot be vetoed: a vetoed
`BuildCancelEvent` on lost land still cancels, with the refund your listener set.

## The domain types

| Type | What it is |
|---|---|
| `BuildJob` | A job: id, owner, package, builder, position, rotation, state, progress, refund, boost |
| `BuildState` | `QUEUED`, `BUILDING`, `PAUSED`, `COMPLETED`, `CANCELLED` and the transient states |
| `CancelReason` | `PLAYER`, `ADMIN`, `PROTECTION_LOST`, `SERVER_ERROR`, `MATERIALS_TIMEOUT`, `RECOVERY`, `SHUTDOWN` |
| `BuildPackage`, `Builder`, `BuilderLevel` | The configured content |
| `Cuboid` | A world name and an inclusive box |
| `Result<T>` | A value, or a message key with placeholders |

Related: [Land protection](features/protection.md), [Integrations](integrations.md)
