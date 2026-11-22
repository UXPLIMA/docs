---
title: Writing a module
order: 183
description: UxmModule, the descriptor, the two contexts, config binding, storage and the registries.
icon: puzzle
---

A module is a class that implements `UxmModule`. uxmClaims loads it like its own modules: it reads the
module's config and language files, creates its tables, registers its commands and menus, and undoes all
of it when the module stops. You compile against `uxmclaims-sdk` (see [Developer](index.md)).

## How uxmClaims finds a module

The modules inside the uxmClaims jar are found through `ServiceLoader`. A module from another plugin is
registered by that plugin while the server loads plugins:

```java
import com.uxplima.uxmclaims.sdk.module.UxmModules;
import org.bukkit.plugin.java.JavaPlugin;

public final class ShopsPlugin extends JavaPlugin {

    @Override
    public void onLoad() {
        UxmModules.register(this, new ShopsModule());
    }
}
```

| Rule | What happens otherwise |
|---|---|
| `depend: [uxmClaims]` in your `plugin.yml` | `UxmModules.register` throws `IllegalStateException`: uxmClaims is not loaded yet |
| Register in `onLoad` | A module registered after uxmClaims started (PlugMan, `/reload`) is ignored with a warning until the next restart |
| The id has a vendor prefix with a dash, such as `example-shops` | The module is marked `INCOMPATIBLE` and does not start. Only uxmClaims' own Skript add-on keeps a plain id (`skript`) |
| The id is not one of uxmClaims' own | Your module is ignored; two third-party modules with one id are both turned off |

Once found, the module appears in `plugins/uxmClaims/modules.yml` and in `/uxm modules`, and the server owner
turns it on or off there like any other module.

## The module class

```java
import com.uxplima.uxmclaims.api.event.land.LandDeleteEvent;
import com.uxplima.uxmclaims.api.model.RoleType;
import com.uxplima.uxmclaims.sdk.module.ModuleContext;
import com.uxplima.uxmclaims.sdk.module.ModuleDescriptor;
import com.uxplima.uxmclaims.sdk.module.ModuleLoadContext;
import com.uxplima.uxmclaims.sdk.module.UxmModule;
import com.uxplima.uxmclaims.sdk.protection.PermissionRegistry;
import com.uxplima.uxmclaims.sdk.protection.RolePermissionSpec;

public final class ShopsModule implements UxmModule {

    private static final ModuleDescriptor DESCRIPTOR = ModuleDescriptor.builder("example-shops")
            .version("1.0.0")
            .depends("lands")
            .softDepends("economy")
            .defaultEnabled(false)
            .foliaSafe(true)
            .build();

    @Override
    public ModuleDescriptor descriptor() {
        return DESCRIPTOR;
    }

    @Override
    public void onLoad(ModuleLoadContext ctx) {
        ctx.config(ShopsConfig.class);
        ctx.schema().sqlResources("db/example-shops");
        ctx.rolePermissions().register(
                RolePermissionSpec.of("example-shops:sell", "action", "emerald", RoleType.OWNER));
        ctx.permissions().node("uxmclaims.command.example-shops.open", PermissionRegistry.Default.TRUE,
                               "Open the shop menu");
    }

    @Override
    public void onEnable(ModuleContext ctx) {
        int max = ctx.config(ShopsConfig.class).get().maxShops;
        ctx.logger().info("Shops per land: " + max);
        ctx.events().subscribe(LandDeleteEvent.class,
                event -> ctx.logger().info("Land gone: " + event.land().name()));
    }
}
```

## Life cycle

| Hook | Runs | Use it for |
|---|---|---|
| `onPluginLoad(PluginLoadContext)` | While the server loads plugins, before any config is read, whether or not the module is on | Only what another plugin accepts only while loading, such as WorldGuard flags |
| `onLoad(ModuleLoadContext)` | For every enabled module, in dependency order | Config, schema, data loaders, flags, role permissions, permission nodes, limits, costs, placeholders |
| `onEnable(ModuleContext)` | In dependency order | Services, listeners, tasks, commands, menus |
| `onAllEnabled(ModuleContext)` | Once every module is running | Looking up services of soft dependencies |
| `onReload(ModuleContext)` | On `/uxm reload` | Re-reading config, language and menu files; commands, tables and registries stay |
| `onDisable(ModuleContext)` | In reverse order | Closing your own resources only |

When `onLoad` or `onEnable` throws, the module is marked `FAILED` and what it registered is undone; the server keeps running.

## The descriptor

| Builder method | Default | Meaning |
|---|---|---|
| `builder(id)` | | Kebab-case, at most 32 characters. Permanent: it names the folder, the tables and the key namespace |
| `version(String)` | `1.0.0` | Shown in `/uxm modules` |
| `sdkVersion(int)` | `1` | The lowest SDK level the module needs; 4.0.0 is level 1. A newer level than the server has makes it `INCOMPATIBLE` |
| `depends(String...)` | none | Modules that must be enabled; otherwise `MISSING_DEPENDENCY` |
| `softDepends(String...)` | none | Modules started first when present |
| `requiredPlugins(String...)` | none | Plugins that must be installed; `floodgate\|Geyser-Spigot` is met by either |
| `required(boolean)` | `false` | Cannot be turned off |
| `defaultEnabled(boolean)` | `true` | Whether a fresh install turns it on |
| `foliaSafe(boolean)` | `false` | Whether it may run on Folia; otherwise `UNSUPPORTED_PLATFORM` there |

## The two contexts

`ModuleLoadContext` (in `onLoad`) has `config`, `messages`, `schema`, `loaders`, `flags`, `rolePermissions`,
`permissions`, `placeholders`, `limits` and `costs`. The flag, role permission, limit and cost registries
close when loading ends. `ModuleContext` (from `onEnable` on) has:

| Method | Gives |
|---|---|
| `config(Class)`, `messages()`, `dataFolder()`, `logger()` | The module's files (`plugins/uxmClaims/modules/<id>/`) and log |
| `api()`, `modules()` | The public API and module states |
| `services()` | `register`, `find`, `require`, `all` for service types; see [The API](api.md) |
| `events()` | `subscribe`, plus `callPre` and `publish` for a module's own events |
| `listeners()` | Bukkit listeners for cold events (joins, chat, deaths) |
| `protection()` | `ProtectionHook`s, asked after bans and before roles for block, entity and move actions |
| `commands()`, `menus()`, `placeholders()` | See [Commands](commands.md) and [Menus](menus.md); placeholders become `%uxmclaims_<prefix>_<key>%` |
| `scheduler()`, `platform()` | Tasks on the right thread on Spigot, Paper and Folia, and the server kind and version |
| `storage()`, `writes()` | Background SQL, and the write pipeline |
| `limits()`, `costs()` | Read a player's limit, quote a price |
| `errors()` | Report an error once a minute, with an id for the player |
| `playerPrefs()`, `displayNames()`, `leadsNetwork()` | Per-player settings, display names of keys, and whether this server runs network-wide jobs |

Never register anything with Bukkit or its scheduler yourself: only what goes through the context is undone
when the module stops.

## Config files

```java
import com.uxplima.uxmclaims.sdk.config.Comment;
import com.uxplima.uxmclaims.sdk.config.ConfigFile;

@ConfigFile("config.yml")
public class ShopsConfig {

    @Comment("Most shops one land may hold.")
    public int maxShops = 5;

    public Prices prices = new Prices();

    public static class Prices {
        @Comment("Price to open a shop.")
        public double createShop = 100.0;
    }
}
```

Public non-final fields bind to kebab-case keys (`maxShops` is `max-shops`, `prices.create-shop`); nested
public static classes are sections; `@Key("default")` names a key the field name cannot. The jar copy lives at
`uxm/modules/<id>/<file>` and is written to `plugins/uxmClaims/modules/<id>/<file>` on first start; missing
keys are added with their comments. `ConfigHandle.get()` always returns the last good copy: a broken file on
reload keeps the old values and logs the wrong key. `onReload(listener)` runs after each good reload.

Raise `@ConfigFile(version = 2)` when you rename or remove keys, and ship the steps in
`uxm/modules/<id>/config-updates/<file>` (`rename`, `set`, `list-add`, `delete` under `steps: 2:`).

Texts go in `uxm/modules/<id>/lang/en/messages.yml` (and other language codes); `messages().send(sender, key,
Placeholder...)` sends one. Message placeholders are written `<key>`; menu files use `%key%`.

## Storage

| Piece | What you do |
|---|---|
| Migrations | `schema().sqlResources("db/<id>")`, with files `db/<id>/<dialect>/V1__init.sql` for `sqlite`, `mysql` (also MariaDB) and `postgresql`. Write table names as `{table:shops}`; they become `uxmc_example_shops_shops` (prefix, then the id with `_` for `-`). An edited migration stops start-up |
| Loading | `loaders().register(tx -> ...)` reads your rows into memory at start-up, before protection opens |
| Writes | `writes().submit(actor, WriteRequest)`: locks, a recheck, the commit in one transaction, then the events you return, on the owning thread |
| Background | `storage().transaction(tx -> ...)` for cleanup and other work no player waits for |

```java
import com.uxplima.uxmclaims.api.action.Actor;
import com.uxplima.uxmclaims.sdk.module.ModuleContext;
import com.uxplima.uxmclaims.sdk.storage.WriteRequest;

import java.util.UUID;
import java.util.concurrent.CompletableFuture;

final class ShopWrites {

    static CompletableFuture<Integer> removeShops(ModuleContext ctx, Actor actor, UUID land) {
        return ctx.writes().submit(actor, WriteRequest.<Integer>builder()
                .lock("land:" + land)
                .commit(tx -> {
                    try (var statement = tx.connection().prepareStatement(
                            "DELETE FROM " + tx.table("shops") + " WHERE land_id = ?")) {
                        statement.setString(1, land.toString());
                        return statement.executeUpdate();
                    }
                })
                .build());
    }
}
```

Do not commit, roll back or close `tx.connection()`; the kernel owns the transaction. `tx.afterCommit(...)`
runs code once the commit succeeded.

## Registries

| Registry | Registers | When |
|---|---|---|
| `flags()` | `FlagSpec.of("example-shops:open", "protection", true, "chest")` | `onLoad` only |
| `rolePermissions()` | `RolePermissionSpec.of(key, category, icon, RoleType...)`; the role types get it on new lands | `onLoad` only |
| `permissions()` | Bukkit nodes with a default and a description, so they show in LuckPerms | `onLoad` |
| `limits()` | `register("uxmclaims.limit.shops", 5, LimitStrategy.MAX)`, read through `uxmclaims.limit.shops.<n>` nodes and `limits.yml` | `onLoad` only |
| `costs()` | `register("example-shops:create", BigDecimal.TEN, "Open a shop")`, priced in the economy module's `costs.yml`; free while economy is off | `onLoad` only |
| `placeholders()` | `resolver("shops", (player, rest) -> ...)`, returning `null` for an unknown key | `onLoad` or later |
| `services()` | A service of your own, or a provider of `RegionGuard`, `MapLayerProvider`, `LandExtensionProvider`, `OwnerTypeHandler` | `onEnable` |

Keys use your module id as namespace. Node names follow `uxmclaims.ability.<category>.<action>`,
`uxmclaims.command.<module>.<sub>` and `uxmclaims.admin.<module>`.

Related: [Commands](commands.md), [Menus](menus.md), [Events](events.md), [modules.yml](../configuration/modules-yml.md)
