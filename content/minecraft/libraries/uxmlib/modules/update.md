---
title: uxmlib-update
order: 29
description: A notify-only release checker with GitHub, Modrinth and JSON providers. It never self-downloads.
icon: refresh-cw
---

Compares the running build against the latest published release, logs it once, and shows a
permission-gated clickable notice on join. It never downloads anything, which is the point: a plugin
that updates itself is a plugin that changes under an operator without their say-so.

## Wiring it up

```java
UpdateChecker checker = new UpdateChecker(
        scheduler, new GitHubReleaseProvider("UXPLIMA", "uxmLib"), getPluginMeta().getVersion());

new UpdateNotifier(plugin, scheduler, checker, "myplugin.update.notify")
        .start(Duration.ofSeconds(40), Duration.ofHours(6));
```

`start(initialDelay, period)` and `stop()` are both idempotent, and `start` may be called again after
a `stop`, so a reload command can restart the poll safely.

The initial delay exists so the check does not compete with everything else enabling at startup.

## Providers

```java
new GitHubReleaseProvider("owner", "repo");
new ModrinthReleaseProvider("projectId");
new JsonUrlReleaseProvider("https://example.com/latest.json");
new JsonUrlReleaseProvider(uri, List.of("version", "tag_name"));
```

`JsonUrlReleaseProvider` reads a version from your own endpoint, with the field names you name. This
is the one to use for a private or paid plugin whose releases are not on a public host.

`UpdateProvider` is a one-method interface returning `CompletableFuture<Optional<Release>>`, so
anything else is a small class.

`provider.endpoint()` reports the URI it will call, which is worth logging once at startup so an
operator can see what the plugin talks to.

## Checking manually

```java
checker.check().thenAccept(outcome -> {
    switch (outcome.status()) {
        case OUTDATED -> logger.info("Update available: " + outcome.release().orElseThrow().version());
        case UP_TO_DATE -> logger.info("Up to date");
        case DEV_BUILD -> logger.info("Running a build newer than any release");
        case FAILED -> logger.warning("Update check failed");
    }
});

checker.lastOutcome();
checker.currentVersion();
```

| Status | Meaning |
|---|---|
| `UP_TO_DATE` | The running build matches or exceeds the latest release |
| `OUTDATED` | A newer release exists |
| `DEV_BUILD` | The running build is newer than anything published |
| `FAILED` | The check could not complete |

`DEV_BUILD` is a real state, not an error. A developer running a local build should not be told they
are out of date.

## Announce once

```java
checker.checkAndAnnounce(outcome -> logger.warning("Update: " + outcome.release().orElseThrow().url()));
```

The callback fires at most once per distinct newer release, not once per poll. A six-hourly timer
therefore produces one console line, and a **later** release announces again because it is a new
version.

## Threading

The provider fetch runs on `scheduler.async`, so the network call never touches a server thread. The
version comparison itself is pure, using `SemanticVersion`, which is what makes it able to say a
stable release is newer than a beta of the same number.

## The join notice

`UpdateNotifier` registers a listener that shows a clickable message to joining players who hold the
permission you passed. It is a permission node, not an operator check, so a server can show it to a
developer role without giving them the world.

<Callout type="note" title="Notify only, deliberately">

There is no self-download and no plan for one. The notice carries the release URL and the operator
decides.

</Callout>
