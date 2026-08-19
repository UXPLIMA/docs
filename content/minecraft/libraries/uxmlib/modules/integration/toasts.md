---
title: Toasts
order: 264
description: The advancement popup, without leaving a permanent advancement behind.
icon: bell
---

A toast is the popup in the top right when a player earns an advancement. Vanilla only produces one
by granting a real advancement, which then sits in the player's advancement list forever.

```java
Toasts toasts = new Toasts(plugin, scheduler);

toasts.builder()
        .icon(Material.DIAMOND)
        .title(Text.mini("<gold>Objective complete"))
        .description(Text.mini("<gray>You found the vault"))
        .frame(AdvancementFrame.GOAL)
        .show(player);
```

The module registers a temporary advancement, grants it, and removes it again, so nothing persists.

| Frame | Look |
|---|---|
| `TASK` | The plain square frame |
| `GOAL` | The rounded frame |
| `CHALLENGE` | The ornate frame, with the challenge sound |

`icon(ItemStack)` uses a full item rather than a bare material, so a custom-model-data icon works.

## Building without showing

```java
Toast toast = Toast.builder()
        .icon(Material.EMERALD)
        .title(Text.mini("<green>Sold"))
        .build();

toasts.show(toast, player);
String json = toast.toJson();
```

Useful when the same toast is shown to many players, or when you want to inspect the generated
advancement JSON.

## Real advancements

```java
Advancements.grant(player, advancement);
Advancements.revoke(player, advancement);
```

For the case where you do want the advancement to stick. Both return whether anything changed.

<Callout type="warning" title="Only a bound builder can show">

`toasts.builder()` returns a builder bound to the service, so `.show(player)` works. `Toast.builder()`
is unbound: it can `build()` a spec but throws if you call `show`, because it has no server services
to register the synthetic advancement with.

</Callout>

## How it works, and why that matters

The module registers a one-criterion synthetic advancement through the data-pack loader, awards that
criterion so the client animates the toast, then revokes and unregisters it a couple of ticks later.
No packets and no NMS.

The keys are namespaced by your plugin name and carry a per-instance random prefix, so two `Toasts`
instances, or one re-created across a reload, can never mint the same key and collide on the
advancement registry.

The player-touching revoke runs on the player's own region thread and the registry removal hops onto
the global region, which is why the service takes a `Scheduler` and never uses a Bukkit one.
