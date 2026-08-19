---
title: Player heads
order: 222
description: The three ways to identify a head owner, and resolving a skin off the main thread without burning the Mojang quota.
icon: user-round
---

## SkullData

A head owner is one of three mutually exclusive things, modelled as a sealed type so they can never
be accidentally mixed.

```java
SkullData.ofUuid(uuid);
SkullData.ofName("Notch");
SkullData.ofTexture(base64);
SkullData.ofUrl("https://textures.minecraft.net/texture/...");

SkullData.parse(userInput);   // works out which of the above it is
```

```java
ItemStack head = ItemBuilder.of(Material.PLAYER_HEAD)
        .skull(SkullData.ofUuid(player.getUniqueId()))
        .build();
```

`parse` is what a config value goes through: an operator writes a name, a dashed or undashed UUID, a
texture URL or a raw base64 texture, and it resolves to the right variant.

<Callout type="warning" title="ByName blocks">

Applying a `ByName` head resolves the name through `Bukkit.getOfflinePlayer(String)`, which is a
blocking lookup. Prefer `ByUuid` or `ByTexture` on the main thread, and use `ByName` only off-thread
or for names the server has already cached.

</Callout>

## Resolving skins asynchronously

A head leaderboard needs thirty textures. Fetching them on the main thread stalls the server;
fetching them naively burns the Mojang quota and gets you rate limited.

```java
SkullResolver resolver = SkullResolver.create(scheduler);

resolver.resolveName("Notch").thenAccept(data ->
        data.ifPresent(texture -> apply(texture)));

resolver.resolveUuid(uuid);
resolver.resolve(SkullData.ofName("Notch"));
```

The blocking lookup runs on the scheduler's async pool. You get a `CompletableFuture` and never block
a server thread.

`create(scheduler)` gives you the live Paper completer, 1024 cached entries and 600 lookups per
minute. The full constructor lets you replace any of those, including the `ProfileCompleter` seam, so
a test resolves against a fake rather than the network.

### What the resolver does for you

**A bounded LRU cache**, keyed by lowercased name or UUID string, so the same head is not fetched
twice.

**Negative caching with a TTL.** An unknown name resolves to empty and that miss is remembered for a
few minutes, then re-fetched. Without the TTL a transient outage would pin a permanent miss until LRU
pressure evicted it.

**Single-flight.** Concurrent lookups of the same key share one in-flight future rather than each
firing a fetch. Thirty players opening the same leaderboard at once is one request.

**Rate limiting.** When no permit is left, the lookup resolves to empty **without caching**, so a
later call can still succeed once the window slides.

## The rate limiter on its own

`RateLimiter` is a sliding-window limiter, useful anywhere you touch an external quota.

```java
RateLimiter limiter = RateLimiter.of(600, Duration.ofMinutes(1));

if (limiter.tryAcquire()) {
    send();
}
limiter.available();
```

Sliding, not fixed-window, so a burst cannot straddle a reset boundary and get double the permits.
The clock is injectable, so the window is testable without sleeping.
