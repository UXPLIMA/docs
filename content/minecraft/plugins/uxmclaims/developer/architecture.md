---
title: Architecture
order: 1001
description: Hexagonal layers, CQRS commands, and why the API looks the way it does.
icon: layers
---

uxmClaims is a hexagonal (ports and adapters) application with a split between reads and writes.
The shape explains why the API is a facade taking command objects rather than a set of setters.

## The modules

| Module | Contains | Depends on |
|---|---|---|
| `uxmclaims-domain` | The model (`Claim` and its parts), the enums, the events, `ClaimException` | nothing |
| `uxmclaims-app` | The facade interfaces, the command objects, the ports, the config models | domain |
| `uxmclaims-core` | The use-case services behind the facades, `ClaimPermissionPolicy` | app, domain |
| `uxmclaims-infra` | The jOOQ repositories, the in-memory cache, migrations, the server lease, locks, cooldowns, webhooks | app, domain |
| `uxmclaims-platform:api` | `UxmClaimBukkitAPI`, `@ClaimEventHandler`, `BukkitConverter`; published as `bukkit-api` | app, domain |
| `uxmclaims-platform:adapters` | The Bukkit adapters for the ports: economy, permissions, regions, placeholders, web maps, the event bus | app, domain, api |
| `uxmclaims-platform:core` | Commands, menus, listeners, wiring | everything above |
| `uxmclaims-platform:dialog-*`, `:bedrock` | Java dialog screens and Bedrock forms, loaded only where the server supports them | `dialog-api` |
| `uxmclaims-platform:bukkit`, `:folia` | The two plugin jars and their schedulers | core |

You compile against `bukkit-api`, which brings `uxmclaims-app` and `uxmclaims-domain` with it. See
[Adding the dependency](dependency.md).

Nothing in `domain` knows Bukkit exists, which is what makes the rules testable without a server.

## Ports and adapters

A port is an interface the application needs. An adapter implements it for one platform or plugin.

| Port | Adapters |
|---|---|
| `EconomyPort` | `VaultEconomy`, `NoopEconomy` |
| `PermissionPort` | `LuckPermsPermission`, `NoopPermission` |
| `RegionPort` | `WorldGuardRegion`, `NoopRegion` |
| `PlaceholderPort` | `PlaceholderAPIPort`, `NoopPlaceholderPort` |
| `MapVisualizerPort` | `DynmapVisualizer`, `BlueMapVisualizer`, `Pl3xMapVisualizer`, `SquareMapVisualizer`, `NoopMapVisualizer` |
| `EventBusPort` | `BukkitEventBus` |
| `ClaimRepositoryPort` | `CompositeClaimRepository` (a `MemoryClaimRepository` cache in front of `JooqClaimRepository`) |
| `LockingPort` / `CooldownPort` | `LocalLockAdapter` / `InMemoryCooldownAdapter` |
| `WebhookPort` | `DiscordWebhook` |

Every integration port has a `Noop` adapter. That is why the plugin runs with none of its soft
dependencies installed: a missing integration is an adapter that does nothing. Which adapter is used
is decided once, at startup; see [Integrations](../integrations/overview.md).

## Commands and queries

Queries are plain methods that read the in-memory cache:

```java
Optional<Claim> claim = claimFacade.findByChunk(BukkitConverter.toDomainChunk(bukkitChunk));
List<Claim> owned     = claimFacade.findByOwnerUid(uuid);
int count             = claimFacade.countByOwnerUid(uuid);
```

Commands are objects describing an intent, handed to the facade:

```java
ClaimRenameCommand command = ClaimRenameCommand.builder()
        .claimId(claim.getId())
        .actorUid(player.getUniqueId())
        .name("New name")
        .build();

Claim renamed = claimFacade.renameClaim(command);
```

The command carries the actor, so one code path enforces permissions, charges the economy, fires the
event and posts the webhook, instead of every caller remembering to.

## The write path

| Step | What happens |
|---|---|
| 1 | Takes a lock on the claim (local to this server, five-second timeout) and loads a copy |
| 2 | Checks permissions, cooldown, balance, limits and regions, as the options allow |
| 3 | Applies the change to the copy |
| 4 | Publishes the [events](events.md); a cancellation aborts here |
| 5 | Charges the actor, then saves in one transaction, refunding if the save fails |
| 6 | Posts the webhook and starts the cooldown |

Each check in step 2 and each action in step 6 is a side effect you can switch off per call with
`ClaimCommandOptions`. This is how an admin tool or a migration script creates claims without
charging anyone, spamming Discord or tripping a limit. The effects and presets are listed in
[The API](api.md).

Because the cache and the locks live inside one server, a database belongs to one server at a time;
see [Database](../database/overview.md).

Related: [The API](api.md), [Events](events.md), [Domain model](models.md)
