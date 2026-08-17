---
title: uxmlib-integration
order: 26
description: Present-guarded hooks, native holograms, toasts and Discord webhooks.
icon: plug
---

Soft-dependency hooks, holograms, toasts and Discord webhooks. Every third-party symbol is touched
only past a plugin-present guard, so a server without the dependency loads cleanly.

## Economy

```java
EconomyBridge.orDummy().deposit(player, 100);
```

Resolves Vault or VaultUnlocked. `orDummy()` returns a no-op implementation when neither is present,
so call sites never null-check and a plugin without an economy degrades to doing nothing rather than
throwing.

`CachedEconomyBridge` caches balances, `AsyncEconomy` moves calls off the main thread, and
`RebindingEconomyBridge` picks up an economy plugin that loads after you do — which is the case that
otherwise produces "no economy found" on a server that has one.

## Permissions and ranks

```java
LuckPermsHook.find()
        .flatMap(lp -> lp.prefix(player))
        .ifPresent(prefix -> applyPrefix(prefix));
```

`find()` returns empty without LuckPerms. `VaultPermission` is the Vault-based alternative.

## Placeholders

```java
String text = Placeholders.apply(player, "Hi %player_name%");
```

A pass-through without PlaceholderAPI installed — the string comes back with its placeholders
unresolved rather than the call failing.

There is a write side too:

```java
PlaceholderRegistry.register(new MyPlaceholderProvider());
```

A `PlaceholderProvider` exposes your own placeholders as `%uxm_<prefix>_<params>%`.
`UxmPlaceholderExpansion` registers the expansion with PlaceholderAPI once, for every provider — so
several plugins can contribute placeholders without each registering their own expansion.

## Regions

```java
RegionHooks regions = new RegionHooks();
WorldGuardRegionService.find().ifPresent(regions::register);

boolean canBuild = regions.active()
        .map(region -> region.canBuild(player, location))
        .orElse(true);
```

WorldGuard and Towny behind one `RegionService` contract. `orElse(true)` is the important part of
that snippet: with no region plugin, the answer is *allowed* — the hook does not become a protection
plugin by accident.

## Holograms

Built on native 1.21+ `Display` entities — Text, Item and Block — with no packets and no per-version
NMS, so they keep working across point releases.

```java
HologramManager holograms = new HologramManager();
holograms.installLifecycleListener(plugin);

Hologram spawn = holograms.spawn(
        Holograms.builder()
                .line(Text.mini("<yellow><bold>Spawn"))
                .line(Text.mini("<gray>Welcome to the server"))
                .billboard(Display.Billboard.CENTER)
                .glow(Color.YELLOW),
        location);
```

`installLifecycleListener` resets per-player state on quit and world change. Without it, per-viewer
holograms accumulate state for players who have left.

On top of the base:

| Type | What it does |
|---|---|
| `HologramPool` | Shows and hides by distance, so a hundred holograms cost what the nearby ones cost |
| `PagedHologram`, `SwitchableHologram` | Per-player widgets the viewer clicks through |
| `LeaderboardHologram` | A live leaderboard with its own renderer |
| `HologramFollow` | Follows an entity |
| `TextAnimation` | Typewriter and scroll effects |
| `PerViewerHologram` | Different content per viewer |
| `ClickableHologram` | Interaction |
| `MojangSkinResolver` | Player heads on head displays |
| `HologramConfig` | Holograms defined in HOCON |

## Toasts

```java
Toast.builder()
        .icon(Material.DIAMOND)
        .title(Text.mini("<gold>Objective complete!"))
        .show(player);
```

A transient advancement toast that leaves no advancement behind. The usual way to do this is to
register a real advancement and revoke it, which pollutes the player's advancement data; this does
not.

## Discord webhooks

```java
new DiscordWebhook(url)
        .sendEmbed(DiscordEmbed.colored("Alert", "Server started", 0x00FF00));
```

No JDA, no bot token, no dependency — a webhook URL and an HTTP call. `EmbedLimits` enforces
Discord's field and length limits before sending, so an over-long message is truncated rather than
rejected.

For anything interactive you need a bot; for one-way notifications this is the whole feature.

## Online data

`OnlineDataManager` and `OnlineDataListener` manage per-player state for the duration of a session —
loaded on join, disposed on quit. It is the lifecycle every plugin writes by hand, once.
