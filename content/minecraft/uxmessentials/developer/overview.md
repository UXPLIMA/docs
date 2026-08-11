---
title: Overview
order: 1610
description: uxmEssentials is built to be a good neighbour. Your plugin can watch
  what it does, refuse the things it is about to do, extend its menus, and read its
  economy, through a published API you compile against like any other library.
---

- **[Adding the dependency](dependency.md)** gives you the coordinate.
- **[Events](events.md)** is the reference for the ninety-odd events, and the handful you can cancel.
- **[Query API](queries.md)** is for asking what is true right now: homes, balances, punishments, who is away.
- **[Action API](actions.md)** is for changing it: set a home, pay a player, hand over a kit, ban somebody.
- **[Menu API](menu-api.md)** is for teaching the GUI engine your own actions, requirements and placeholders.
- **[REST API](rest-api.md)** is the same reach over HTTP, for a panel, a bot or a script that is not a plugin.

## The front door

`UxmEssentialsApi` is where everything starts. There are two ways to reach it, because the two failure modes are
different.

If your plugin is guaranteed to enable after uxmEssentials, Bukkit's service registry is enough:

```java
UxmEssentialsApi api = getServer().getServicesManager().load(UxmEssentialsApi.class);
if (api == null) {
    getLogger().info("uxmEssentials is absent; running without it");
    return;
}
```

If load order is not guaranteed, which on somebody else's server it never really is, use the callback instead. It
runs immediately when uxmEssentials is already up, waits when it is not, and runs again after a reload so your
registrations are restored:

```java
public final class MyAddon extends JavaPlugin {

    @Override
    public void onEnable() {
        getServer().getPluginManager().registerEvents(new MyListener(), this);

        UxmEssentialsApi.whenReady(api -> {
            getLogger().info("uxmEssentials " + api.version() + " is ready");
            api.menus().registerAction("my-award", click -> click.player().giveExp(100));
        });
    }
}
```

<Callout type="tip" title="Listeners need none of this">

Registering a listener does not require the API to be present or loaded. Bukkit resolves events by class, so a
listener registered in your `onEnable` receives uxmEssentials events whether it loaded before or after. Use
`whenReady` for the things that genuinely need the running plugin, such as menu registrations.

</Callout>

`UxmEssentialsApi.get()` is the third form, and it is honest about its answer: it returns `null` when uxmEssentials is
absent, still loading, or shutting down. Nothing here throws merely because the plugin is missing.

## What the API covers today

| Surface | State | Where |
|---|---|---|
| Events, for everything the plugin does | Available | [Events](events.md) |
| Veto (cancellable pre-events) for the operations that can be refused cleanly | Available | [Events](events.md#what-you-can-cancel) |
| Menu extension: actions, requirements, placeholders, list sources, icons | Available | [Menu API](menu-api.md) |
| Reading data directly, across sixteen contexts | Available | [Query API](queries.md) |
| Performing operations directly, across thirteen contexts | Available | [Action API](actions.md) |
| HTTP and WebSocket access for programs that are not plugins | Available as a separate add-on jar | [REST API](rest-api.md) |
| Economy | Available through Vault and Treasury | [Vault and Treasury](../integrations/vault-treasury.md) |

Economy deliberately has no bespoke port. uxmEssentials registers itself into the ecosystem's standard slots,
Treasury first and then Vault, so if you already talk to `net.milkbowl.vault.economy.Economy` you are already talking
to uxmEssentials when it is the active provider.

## Modules that are switched off

uxmEssentials is a set of modules an operator can turn off one at a time, and [nine of them ship switched
off](../modules/overview.md). A disabled module fires no events and holds no state, which from the outside looks
exactly like a module that is simply idle.

Ask, rather than infer:

```java
if (api.isModuleEnabled("homes")) {
    // the home events will fire
}
```

The id is the one the operator writes in `modules.conf`.

## Threading

uxmEssentials does its work off the tick thread, because it talks to a database, and it supports Folia, where there is
no single main thread to return to. Two rules follow, and [Events](events.md#threading) covers them in detail:

- **Notification events** are delivered on the tick thread that owns their subject. Use the Bukkit API freely.
- **Pre-events**, the cancellable ones, reach you on whatever thread the operation is on, which is usually not a tick
  thread. Read the event, decide, return. Do not touch the Bukkit API from those handlers.

## Versioning

The API follows the plugin's version. Within a major version, published types and methods are added but not removed
or renamed: a plugin compiled against an earlier release keeps working on a later one.

Two guards in the build hold that promise rather than leaving it to memory. The full surface, every public type and
signature, is written down and compared on each build, so a removal shows up as a deleted line in review. A sample
consumer plugin is compiled in CI against the artifacts each commit publishes, so a coordinate or a POM that stops
working fails on the commit that broke it rather than on somebody's server months later.

## Next steps

- [Adding the dependency](dependency.md)
- [Events](events.md)
- [Action API](actions.md)
- [Menu API](menu-api.md)
- [REST API](rest-api.md)
