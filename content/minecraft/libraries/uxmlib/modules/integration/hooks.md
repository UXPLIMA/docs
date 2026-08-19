---
title: Hooks
order: 261
description: Economy, permissions, placeholders and region queries behind provider-agnostic contracts, plus lazy binding for plugins that enable after you.
icon: unplug
---

## Presence

```java
Hooks.isPresent("PlaceholderAPI");
```

The one primitive everything else is built on.

## The hook registry

A hook may depend on a plugin that enables after yours, so binding cannot always happen in
`onEnable`.

```java
HookRegistry hooks = new HookRegistry();

hooks.register(EconomyBridge.class, EconomyBridge.orDummy());
hooks.defer(RegionService.class, "WorldGuard", () -> WorldGuardRegionService.find().orElseThrow());

hooks.bindPresent();                      // sweep plugins already enabled, at startup
hooks.onPluginEnabled(event.getPlugin().getName());   // from a PluginEnableEvent listener

Optional<RegionService> regions = hooks.get(RegionService.class);
```

An instance, not static state, so each plugin owns its own registry.

## Economy

```java
EconomyBridge economy = EconomyBridge.orDummy();

economy.deposit(player, 100);
economy.withdraw(player, 50);
economy.has(player, 50);
economy.balance(player);
economy.format(1234.5);
economy.isPresent();
economy.currencySymbol();
economy.currencyNameSingular();
economy.currencyNamePlural();
```

`orDummy` picks the best backend available, classic Vault then VaultUnlocked, and falls back to a
no-op `DummyEconomy` so a call site never null-checks. `EconomyBridge.find()` returns the `Optional`
when you want to know.

<Callout type="warning" title="Economy calls may block">

They run through whatever economy plugin is installed, and some of those hit a database
synchronously. Route them off the main thread.

</Callout>

### Async and caching wrappers

```java
AsyncEconomy async = new AsyncEconomy(economy, scheduler);

async.balanceAsync(player).thenAccept(this::show);
async.hasAsync(player, 50);
async.withdrawAsync(player, 50);
async.depositAsync(player, 100);
```

```java
EconomyBridge cached = new CachedEconomyBridge(economy, Duration.ofSeconds(5), Clock.systemUTC());
cached.invalidate(player);
cached.invalidateAll();
```

A cached bridge is for a menu that shows a balance in twenty slots. Invalidate after any write your
own code makes.

### Surviving an economy reload

```java
RebindingEconomyBridge bridge = new RebindingEconomyBridge();
Bukkit.getPluginManager().registerEvents(new EconomyServiceListener(bridge), plugin);
```

Watches Bukkit's service registry and rebinds when an economy plugin registers or unregisters, so a
`/plugman reload` of Vault does not leave your plugin holding a dead provider.

## Permissions and ranks

```java
LuckPermsHook.find().ifPresent(lp -> {
    lp.prefix(player);
    lp.suffix(player);
    lp.primaryGroup(player);
    lp.metaValue(player, "town");
    lp.groups(player);
    lp.groupExists("vip");
});
```

Offline lookups are asynchronous, because they are not cached and must not block:

```java
lp.prefixAsync(uuid);
lp.suffixAsync(uuid);
lp.primaryGroupAsync(uuid);
lp.metaValueAsync(uuid, "town");
```

The Vault permission bridge covers the generic case:

```java
VaultPermission.find().ifPresent(perms -> {
    perms.has(player, "myplugin.use");
    perms.add(player, "myplugin.trial");
    perms.remove(player, "myplugin.trial");
    perms.primaryGroup(player);
});
```

## Placeholders

Reading is a pass-through that does nothing harmful without PlaceholderAPI installed:

```java
Placeholders.isAvailable();
String text = Placeholders.apply(player, "Hi %player_name%");
```

Writing exposes your own placeholders:

```java
PlaceholderRegistry registry = new PlaceholderRegistry()
        .register("homes", (player, params) -> String.valueOf(homeCount(player)));

PlaceholderExpansions.register(registry, "UXPLIMA", "1.0.0");
PlaceholderExpansions.register("myplugin", registry, "UXPLIMA", "1.0.0");
```

Your placeholders are then `%uxm_homes_<params>%`, or `%myplugin_homes_<params>%` with the explicit
identifier form. Registration returns `false` rather than throwing when PlaceholderAPI is absent.

## Regions

One contract over WorldGuard and Towny, so your code does not care which is installed, or whether
either is.

```java
RegionHooks regions = new RegionHooks();
WorldGuardRegionService.find().ifPresent(regions::register);
TownyRegionService.find().ifPresent(regions::register);

boolean canBuild = regions.active()
        .map(service -> service.canBuild(player, location))
        .orElse(true);
```

| Method | Question |
|---|---|
| `canBuild(player, location)` | May they build here |
| `canInteract(player, location)` | May they interact here |
| `regionsAt(location)` | Which regions cover this point |
| `isWilderness(location)` | Is this unclaimed |
| `isAvailable()` | Is the backing plugin still there |
| `pluginName()` | Which provider this is |

`regions.hasProvider()` tells you whether anything registered. Defaulting to `true` when nothing did,
as above, is usually right: no region plugin means no region restrictions.
