---
title: uxmlib-update
order: 29
description: A notify-only release checker that never downloads anything.
icon: refresh-cw
---

A release update checker that notifies and never downloads.

```java
UpdateChecker checker = new UpdateChecker(
        scheduler,
        new GitHubReleaseProvider("you", "your-plugin"),
        UxmLibVersion.VERSION);

new UpdateNotifier(plugin, scheduler, checker, "yourplugin.update.notify")
        .start(Duration.ofSeconds(40), Duration.ofHours(6));
```

`start(delay, interval)` — first check forty seconds after startup, then every six hours. The delay
matters: checking during `onEnable` competes with everything else the server is doing to start.

## Providers

| Provider | Source |
|---|---|
| `GitHubReleaseProvider` | GitHub releases for an owner and repository |
| `ModrinthReleaseProvider` | A Modrinth project |
| `JsonUrlReleaseProvider` | Any JSON endpoint you control |

The comparison uses `SemanticVersion` from `uxmlib-common`, against a build-time version constant —
not the plugin's runtime version, which a repackaged jar can lie about.

## Notification

`UpdateNotifier` logs to the console and shows a clickable message on join to players holding the
permission you passed. Everyone else sees nothing.

Permission-gating this is not cosmetic. An update notice tells anyone reading it exactly which
version of which plugin the server is running, which is the first thing an attacker wants to know.

## Notify-only, on purpose

It never self-downloads and never replaces a jar. Automatic updating means a plugin can change under
a server owner without their knowledge, and a compromised release channel becomes remote code
execution on every server running it.

The notification carries the version and the link. The decision to update stays with whoever runs
the server.

## Outcomes

`UpdateOutcome` and `UpdateStatus` describe what a check found — up to date, an update available, or
the check failed. A failed check is a normal outcome, not an exception: GitHub rate limits, and a
plugin should not log a stack trace because a network call did what network calls do.

`UpdateMessages` holds the message templates, so the notice is translatable.
